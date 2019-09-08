---
layout: post
title: "OpenStack Virtual Machine On Ceph Crash After Power Loss [Part 1]"
date: 2019-09-07 18:58:01
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

Chào các bác, lại là tôi @daikk115 đây!

---
Tóm tắt

Vấn đề gây ra bởi invalid lock của Ceph image, cần remove lock cũ đi và reboot máy ảo.

  ```bash
  [root@myserver ~]# rbd lock ls vms/0cab75e6-5cb0-46ff-a8ac-872f1a5c6023_disk
  There is 1 exclusive lock on this image.
  Locker           ID                  Address
  client.247662578 auto 94541455753216 10.240.201.139:0/2318786000

  [root@myserver ~]# rbd lock rm vms/0cab75e6-5cb0-46ff-a8ac-872f1a5c6023_disk "auto 94541455753216" client.247662578
  ```

---

Chả là sau mấy năm làm hệ thống thì tôi nhận thấy là việc cầu nguyện là thực sự cần thiết. Tuy nhiên, máy chủ thì vẫn cữ lỗi bất thình lình và ảnh hưởng của nó thì cũng vô số trường hợp có thể xảy ra.

![Pray for server]({{ site.baseurl }}/assets/img/pray_for_server.jpg)

Source: <http://www.netgain-systems.com/wp-content/uploads/2017/08/Server-blessing-Poland.jpg>

Một trong số các trường hợp mà tôi gặp phải là việc sử dụng Ceph làm backend cho OpenStack rồi một ngày compute node bất chợt bị mất nguồn hoặc bị cố tình power off từ trên giao diện quản trị. Hậu quả dẫn tới là các máy ảo trong đó bị crash. Hôm nay, tôi xin phép chia sẻ tới các bác lỗi này và nguyên nhân thực sự đằng sau vụ lỗi này.

À quên, phiên bản tôi đã và đang sử dụng mà có gặp lỗi trên như sau:

- Ceph: Jewel & Mimic
- OpenStack: Queens & Rocky

Quả thực, lúc đầu tôi nghĩ là do cache của rbd trên compute node vì nghĩ compute node tắt đột ngột quá là file system bị lỗi chăng. Nhưng lỗi kiểu này thì bình thường `fsck` là xong ấy thế mà lại không áp dụng để fix được. Chúng tôi đã thử tăng giảm cache để thử nghiệm vì nghĩ chắc mình chưa tuning gì nên nó lởm, cũng vẫn không được!

Sau đó, tôi có tham khảo anh em trong cộng đồng OpenInfra tại Việt Nam và các trang mailing list về Ceph cũng như OpenStack thì được biết là rất nhiều người gặp lỗi này và cũng chưa có câu trả lời thỏa đáng.

Bẵng đi một thời gian vì task dồn đống nên tôi chưa nghiên cứu tiếp được. Trong lúc này, chúng tôi sử dụng cách rebuild object-map của Ceph image cho mỗi lần compute bị đột tử và nó hiệu quả ^^

  ```bash
  [root@myserver ~]# rbd object-map rebuild volumes/volume-5bd54eb9-b181-4d10-b6cd-8ae6841edcf5
  Object Map Rebuild: 100% complete...done.
  ```

Dạo gần đây, tôi tìm hiểu thêm thì được biết anh em sử dụng OpenShift, Ceph container hay Rock cũng bị và một cách để tránh việc lỗi này là `disable exclusive-lock` của image. Chợt nhớ ra là OpenStack cũng tạo Ceph image và enable exclusive-lock. Tôi quyết định bắt tay ngay vào việc debug và tìm hiểu xem OpenStack và Ceph thì có gì đặc biệt, kết quả như sau:

- Trước khi reset compute node từ giao diện quản trị

  ```bash
  [root@myserver ~]# rbd lock ls vms/0cab75e6-5cb0-46ff-a8ac-872f1a5c6023_disk
  There is 1 exclusive lock on this image.
  Locker           ID                  Address
  client.247961907 auto 94049029718016 10.240.201.139:0/2548741492
  ```

- Reset compute node, máy ảo bị crash, kể cả hard reboot cũng không giải quyết được --> Lúc này lock của image vẫn y chang như cũ.

- Thực hiện remove lock và hard reboot --> Máy ảo hoạt động trở lại bình thường

  ```bash
  rbd lock rm vms/0cab75e6-5cb0-46ff-a8ac-872f1a5c6023_disk "auto 94541455753216" client.247662578
  ```

Ngoài ra, lock của các Ceph image (hay OpenStack volume) cũng thay đổi mỗi khi soft/hard reboot máy ảo OpenStack.

- Trước và sau khi hard reboot

  ```bash
  [root@myserver ~]# rbd lock ls vms/45a318d6-17b3-49ad-a5f0-8c69a5c5667c_disk
  There is 1 exclusive lock on this image.
  Locker           ID                  Address
  client.247695590 auto 94122114498560 10.240.201.139:0/214440748

  [root@myserver ~]# rbd lock ls vms/45a318d6-17b3-49ad-a5f0-8c69a5c5667c_disk
  There is 1 exclusive lock on this image.
  Locker           ID                  Address
  client.248021391 auto 93872108527616 10.240.201.139:0/72018164
  ```

- Trước và sau khi soft reboot

  ```bash
  [root@myserver ~]# rbd lock ls vms/45a318d6-17b3-49ad-a5f0-8c69a5c5667c_disk
  There is 1 exclusive lock on this image.
  Locker           ID                  Address
  client.248021391 auto 93872108527616 10.240.201.139:0/72018164

  [root@myserver ~]# rbd lock ls vms/45a318d6-17b3-49ad-a5f0-8c69a5c5667c_disk
  There is 1 exclusive lock on this image.
  Locker           ID                  Address
  client.247698080 auto 94867447029760 10.240.201.139:0/1055129882
  ```

Như vậy, có thể thấy với các trường hợp bình thường, OpenStack hoàn toàn có thể  điều khiển để release lock cũ và cấp phát lock mới. Đối với trường hợp compute node bị lỗi, chúng ta sẽ phải thực hiện bằng tay xóa bỏ lock cũ đi và reboot máy ảo, máy ảo sẽ hoạt động lại bình thường.

  ```bash
  [root@myserver ~]# rbd lock rm vms/0cab75e6-5cb0-46ff-a8ac-872f1a5c6023_disk "auto 94541455753216" client.247662578
  ```

Nếu các bác đã đọc tới đây và có đồng cảm vì cũng gặp lỗi này hoặc bất kỳ trao đổi gì thêm, xin gửi mail về địa chỉ: daikk115@gmail.com.

Peace,

Daikk115
