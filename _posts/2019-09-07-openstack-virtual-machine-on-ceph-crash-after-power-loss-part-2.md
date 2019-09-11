---
layout: post
title: "OpenStack Virtual Machine On Ceph Crash After Power Loss [Part 2]"
date: 2019-09-09 22:49:33
image: '/assets/img/'
type: post
published: true
status: publish
tags:
- ceph
- openstack
categories:
- Chia sẻ kinh nghiệm
author:
  login: Daikk115
  email: daikk115@gmail.com
  display_name: Daikk115
  first_name: Dai
  last_name: Dang Van
---

Chào các bác, lại là tôi @daikk115 đây! Tôi muốn viết part 2 ngay sau hôm viết part 1 nhưng lại bận nên delay mấy hôm giờ mới xong :D

---
Tóm tắt

- Cần cấp quyền clear blacklisted client (allow command 'osd blacklist') cho user Ceph cấu hình trên OpenStack
- Khi một client bị cho vào blacklist, quyền ghi vào Ceph image sẽ không được cấp phát cho client
- Có hai cách để phần quyền như sau:

  ```ini
    [client.nova]
        key = AQAqMlhbB3SXGRAAUT9/7ZSTBmJRlG+i03VDxg==
        caps mon = "allow r, allow command 'osd blacklist'"
        caps osd = "allow class-read object_prefix rbd_children, allow rwx pool=volumes, allow rwx pool=vms, allow rwx pool=images"

    [client.nova]
        key = AQAqMlhbB3SXGRAAUT9/7ZSTBmJRlG+i03VDxg==
        caps mon = "profile rbd"
        caps osd = "profile rbd pool=volumes, profile rbd pool=vms, profile rbd pool=images"
  ```

- Refer link: <https://access.redhat.com/solutions/3391211>

---

Trong phần 1, tôi đã nói về nguyên nhân gây lỗi xung quanh chỗ lock của Ceph image và việc xóa bỏ lock để giải quyết tạm thời vấn đề. Lock của Ceph image này là tính năng support bởi `exclusive lock feature` của Ceph image (ae có thể google thêm để hiểu về lịch sử ra đời cũng như chức năng/cách dùng của nó). Về cơ bản, `exclusive lock` đảm bảo cho việc chỉ có duy nhất một client được phép ghi vào Ceph image tại một thời điểm trong khi có thể có nhiều client khác cũng đang trỏ tới image đó.

> Mọi người có thể test bằng cách dùng lệnh rbd map để map cùng 1 image lên 2 client, xong rồi format và mount đồng thời từ cả 2 client sẽ thấy chức năng này hoạt động như thế nào

Liên quan tới vấn đề compute node reboot, về phần Ceph, khi compute bị reboot hay hypervisor bị lỗi hay bất kỳ nguyên nhân gì khiến cho kết nối giữa máy ảo tới Ceph bị gián đoạn hay gói tin không toàn vẹn, client/IP của compute node sẽ bị xem xét cho vào blacklist để đảm bảo an toàn cho dữ liệu. Tại sao?

- Việc kết nối bị đột ngột gián đoán, khiến Ceph không biết được client đó còn sống hay đã chết
- Và dữ liệu có thể chưa flush hết xuống phía Ceph, client sẽ được blacklist và lock sẽ không được cấp phát cho client khác vì có thể làm cho dữ liệu không nhất quán (race condition)

> Việc một client ở trạng thái nằm trong blacklist hay không sẽ tương ứng với việc được cấp phát hoặc renew lock khi không có client khác đang chiếm lock

Vậy nếu như mình biết chắc chắn client đã hoạt động trở lại bình thường sau khi kết nối tới Ceph bị gián đoạn, (giống như cách mà compute node bị reboot, máy ảo chuyển về trạng thái shutdown) chúng ta cần làm gì? Chúng ta sẽ cần thực hiện thủ công hành động gỡ bỏ client đó khỏi blacklist!

- Một kết nối tới một Ceph image bị cho vào blacklist và lệnh remove blacklist

    ```bash
    [root@myserver ~]# ceph osd blacklist ls
    listed 1 entries
    10.240.201.139:0/2318786000 2019-09-09 10:19:26.795449

    [root@myserver ~]# ceph osd blacklist rm 10.240.201.139:0/2318786000
    un-blacklisting 10.240.201.139:0/2318786000

    [root@myserver ~]# ceph osd blacklist ls
    listed 0 entries
    ```

Về phía OpenStack compute node, nó đã được phát triển để thông qua Ceph client sẽ gỡ bỏ chính nó ra khỏi blacklist sau khi compute node bị reboot (Vì OpenStack biết một máy ảo đang ở node compute nào, nó hiểu rằng nên cho phép node compute có IP bao nhiêu truy cập ổ đĩa ảo/Ceph image tương ứng với máy ảo). Tuy nhiên, vấn đề chính là `tôi đã cấp thiếu quyền cho Ceph user`, vì vậy khi OpenStack compute node bị reboot đột ngột, client/IP node compute này đã bị cho vào blacklist và máy ảo đang sử dụng ổ đĩa ảo trên Ceph sẽ không được cấp lock mới khi khởi động --> Không boot lên được.

- Quyền truy cập tôi đã thiết lập trước khi bị lỗi như sau:

    ```ini
        [client.nova]
            key = AQAqMlhbB3SXGRAAUT9/7ZSTBmJRlG+i03VDxg==
            caps mon = "allow r"
    ```

- Quyền truy cập tôi đã thiết lập lại đầy đủ sau khi phát hiện lỗi như sau:

    ```ini
        [client.nova]
            key = AQAqMlhbB3SXGRAAUT9/7ZSTBmJRlG+i03VDxg==
            caps mon = "allow r, allow command 'osd blacklist'"
    ```

Để khi reboot compute node không bị lỗi như tôi đã chia sẻ ở Part 1, cần thêm `allow command 'osd blacklist'` vào cho mon caps.

- Log lỗi khi tôi cung cấp thiếu quyền

    ```bash
    [INF] from='client.? 10.240.201.139:0/2504086678' entity='client.nova' cmd=[{"prefix": "osd blacklist", "blacklistop": "add", "addr": "10.240.201.139:0/104874969"}]:  access denied
    ```

- Log bình thường xuất hiện khi máy ảo được bật lại sau khi compute bị restart nhưng với user được cấp phát đầy đủ quyền

    ```bash
    [INF] from='client.249288019 -' entity='client.nova' cmd=[{"prefix": "osd blacklist", "blacklistop": "add", "addr": "10.240.201.139:0/104874969"}]: dispatch
    [INF] from='client.249288019 -' entity='client.nova' cmd='[{"prefix": "osd blacklist", "blacklistop": "add", "addr": "10.240.201.139:0/104874969"}]': finished
    ```

Ngoài ra, theo như trang chủ của Ceph từ bản Mimic, chúng ra được khuyến nghị cấu hình theo cap profiles để đơn giản hơn cho việc cấp phát quyền truy cập.

- Cấu hình như sau

    ```ini
        [client.nova]
            key = AQAqMlhbB3SXGRAAUT9/7ZSTBmJRlG+i03VDxg==
            caps mon = "profile rbd"
            caps osd = "profile rbd pool=volumes, profile rbd pool=vms, profile rbd pool=images"
    ```

- Refer link: <https://docs.ceph.com/docs/mimic/rbd/rbd-openstack/>

`Note`: Tại liệu trên trang chủ Ceph bản Jewel ( <https://docs.ceph.com/docs/jewel/rbd/rbd-openstack/>) đang cấu hình theo các cũ và thiếu đi phần `allow command 'osd blacklist'`. Chính chúng tôi cũng đã cài đặt theo hướng dẫn này trước khi nâng cấp lên bản Mimic và tiếp tục bị lỗi như trên.

Cảm ơn các bác đã đọc tới đây :D Nếu các bác bất kỳ trao đổi gì thêm, xin gửi mail về địa chỉ: daikk115@gmail.com. À, nếu có bác nào cũng dính lỗi như tôi và muốn cập nhật lại quyền cho các user, các câu lệnh sau có thể sẽ hữu ích:

- Export client keyring

    ```bash
    ceph auth get client.nova -o ceph.client.nova.keyring
    ceph auth get client.cinder -o ceph.client.cinder.keyring
    ```

- Cập nhật lại phần mon cap bằng cách chỉnh sửa file và thực hiện import ngược lại Ceph

    ```bash
    ceph auth import -i ceph.client.cinder.keyring
    ceph auth import -i ceph.client.nova.keyring
    ```

Peace,

Daikk115
