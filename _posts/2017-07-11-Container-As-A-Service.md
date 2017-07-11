---
layout: post
title: CaaS-Requirements
date: 2017-07-11
type: post
published: true
status: publish
categories:
- Tech
meta:
  _wpcom_is_markdown: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
author:
  login: vnstack
  email: vietstack@gmail.com
  display_name: vietstack
  first_name: Viet
  last_name: Stack
---


## GENERAL REQUIREMENTS:

Lưu ý:  Trong note này,  từ  "host"  được dùng  để ám chỉ đến các máy ảo trong môi trường cloud mà các containers đang chạy trên.


1.Quản lý các resources  (RAM, CPU, etc.)  theo các  ứng dụng và tenant:

- Một container cluster có thể chạy một hoặc  nhiều ứng dụng.

- Các ứng dụng có  thể yêu cầu các resources  (số lượng CPU, RAM, etc.)  và CaaS  phải đảm bảo rằng  việc cung cấp các resources này  được thực hiện.

- Các  tenant  (quản lý một tập hợp các containers) có thể set quota về resources cho các ứng dụng của mình chạy trên các containers, đảm bảo rằng các ứng dụng khác không ảnh hưởng đến  hoạt động của mình trong trường hợp cùng một container  cluster,  có rất nhiều ứng dụng chạy trên đó.

2.Khả năng  liên  kết  tới  network

- Container phải có khả năng  kết nối với nhiều network.  Sẽ có các templates của CaaS được dùng để định nghĩa/chỉ định một network cụ thể (trong nhiều network của infrastructure)  dùng cho việc kết nối tới container trên.

- Nếu một container  cần  thiết phải được kết nối tới nhiều networks thì nó phải được kết nối với mô hình 1:1  (một địa chỉ IP/một network).

3.Khả năng liên kết tới lưu trữ

- Container trong  kiến  trúc microservice  được  sử dụng hầu hết cho  các ứng dụng  stateless. Tuy  nhiên không  có  nghĩa  là  container không được dùng cho  các  ứng dụng stateful. Do đó CaaS  phải có khả  năng  hỗ trợ việc cung cấp các permanent volume được gán (attached) vào các  container để  lưu các dữ liệu cần thiết.

- Từ yêu cầu trên thì  CaaS phải có khả năng cung  cấp  volume  với kích cỡ  được  yêu cầu  (có thể thông qua template)  và được gán  vào  container thông qua  mount point.

- Một số containers có thể cần  một shared storage,  do đó CaaS  cần phải hỗ trợ việc  nhiều containers có thể truy cập  vào cùng một shared storage.

4.Container Orchestration/Management  trong thời gian thực (real-time)

- Một trong những nhiệm vụ chinh của CaaS  chính là việc quản lý và triển khai các container trong thời gian thực. 

- Việc scheduling các  containers này cần phải  support các yêu cầu về anti-/affinity policy hay về cluster.

5.Khả năng lưu trữ các image một cách an toàn.

- Các images  cần phải được lưu ở trong registry của CaaS  để khi cần CaaS có thể pull về một cách nhanh chóng.  Ngoài ra CaaS cần phải có các mechanism để kiểm tra/đảm bảo tính  toàn vẹn của images,  tránh việc images bị hỏng  (compromised, corrupted).

6.Rolling upgrade

- CaaS được dùng để deploy các app trên nền container,  do đó nếu  ta có một version mới của các app,  các images  mới sẽ được lưu vào trong registry của CaaS và CaaS phải có khả năng upgrade các container instances  sao cho các services vẫn hoạt động  bình thường.

- Ngoài ra CaaS cũng phải hỗ trợ việc down-grade  các images nếu  cần.

7.Khả  năng self-healing vàà  khả năng co giãn

- Khi các containers  không hoạt động do một số vấn đề như  host (nơi mà các containers đang chạy trên)  hoặc các apps chạy trên container gặp sự cố thì CaaS phải hỗ trợ việc healing các container đó,  e.g.  deploy các containers tương tự trên các host khác.

- Ngoài ra CaaS cũng phải hỗ trợ việc scaling số lượng các containers dựa theo các yêu cầu,  policies hoặc dựa theo kết quả từ monitoring. 

8.Khả  năng  inter-communication trong môi trường cloud

- CaaS cần phải có interface để giao tiếp  với các  services khác trong môi trường cloud  phục  vụ cho các tác vụ liên quan đến life-cycle của containers,  e.g.  interface tương tác với host manager service  để  trong trường hợp một host nào đó đang chứa ccác  containers không hoạt động, CaaS cần phải báo cho  host manager service để stop/migrate/heal host đó.

9.Khả năng support baremetal và virtual machine 

- Điều  này đồng  nghĩa  với  việc CaaS  hỗ  trợ việc orchestrating  containers trên  môi  trường cloud  và baremetal.


10-7-2017
VietStack team
