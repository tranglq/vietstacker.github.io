---
layout: post
title: "Best Practice Ansible Configuration for Kolla"
date: 2019-09-07 18:15:15
image: '/assets/img/'
type: post
published: true
status: publish
tags:
- kolla-ansible
- ansible
- openstack
categories:
- Chia sẻ kinh nghiệm
author:
  login: Daikk115
  email: daikk115@vietopeninfra.org
  display_name: Daikk115
  first_name: Dai
  last_name: Dang Van
---

Chào các bác, lại là tôi @daikk115 đây!

---
Tóm tắt:

- Tối ưu cấu hình ansible khi chạy kolla-ansible
- Hot-fix kolla-ansible

---

Từ đợt viết loạt bài về Rolling Upgrade xong tôi bị lụt trong một chuỗi dự án dài hơi nên quên mất nhiệm vụ viết bài chia sẻ :D Nay tranh thủ công ty cho các bé đi chơi trung thu, các trai thanh nữ tú bận bịu để có nhiều trẻ em hơn, mà tôi thì rảnh cộng thêm cảm giác tội lỗi lâu rồi không viết gì nên về nhà sớm viết bài gửi các bác đây!

Dự án OpenStack Kolla là một trong các nỗ lực của cộng đồng OpenInfra nhằm tối ưu hóa quá trình triển khai OpenStack và các thành phần phụ trợ cũng như giảm thiếu được nhiều vấn đề  trong quá trình vận hành Cloud bằng việc đóng gói các dịch vụ (OpenStack services, Monitor services, 3rd services) dưới dạng Docker container. Đồng thời, bởi sự hữu ích của nó mà Kolla đã nhận được sự đóng góp mạnh mẽ từ đông đảo thành viên trong cộng đồng OpenInfra trên toàn cầu trong đó có cả các thành viên cộng đồng OpenInfra tại Việt Nam.

Về mặt công nghệ, dự án Kolla phát triển dựa trên hai công cụ chính đó là Docker và Ansible. Trong đó, việc triển khai các dịch vụ sẽ được tự động hóa bằng Ansible. Với Ansible thì các thao tác tay chân lặp đi lặp lại đã gần như được giải quyết hoàn toàn, tuy nhiên với môi trường hàng trăm máy chủ vật lý thì cũng cần tối ưu một chút về Ansible để nó ngon và mượt mà hơn. Dưới đây là một số cấu hình chúng tôi đang sử dụng để triển khai nhanh hơn với Kolla-Ansible.

#### Cấu hình cho ansible ở file `/etc/ansible/ansible.cfg`

  ```ini
  [defaults]
  host_key_checking = False
  forks = 39
  gathering = smart
  fact_caching = jsonfile
  fact_caching_connection = /etc/ansible/facts.d
  fact_caching_timeout = 0

  [ssh_connection]
  ssh_args = -o ControlMaster=auto -o ControlPersist=900s
  pipelining = True
  ```

Trong đó:

- forks: số lượng thread chạy đồng thời (nên để số lượng thread = tổng số CPU - 1)
- gathering: gather facts theo mode smart để sử dụng cache plugin
- fact_caching: cache dưới dạng file json
- fact_caching_connection: đường dẫn thư mục chứa file cache
- fact_caching_timeout: thời gian cache timeout (để 0 thì không bao giờ timeout)
- ssh_connection: cấu hình keep ssh connection

#### Hot-fix kolla-ansible

- Khi đã có cache, bỏ qua play `Gather facts for all` trong kolla-ansible
- Có thể skip các fact không liên quan như hardware

  ```yaml
  setup:
    gather_subset: !hardware
  ```

- Mặc định, kolla-ansible sẽ luôn gọi common role từ tất cả các role. Khi deploy mà không ảnh hưởng đến các thành phần common, skip common trong metadata của role tương ứng
- Khi deploy nhiều dịch vụ có chứa common, có thể skip common trong các tag đầu, chỉ để lại common trong tag cuối nhằm tránh chạy đi chạy lại nhiều lần việc triển khai các container thuộc common role

Peace,

Daikk115
