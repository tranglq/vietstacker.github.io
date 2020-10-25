---
layout: post
title: "Tổng quan về Mountebank mock server và hướng dẫn cài đặt"
date: 2020-10-25 22:49:33
image: '/assets/img/'
comments: true
tags:
- mountebank
categories:
- news
- tech
author:
  login: Tranglua
  email: lequynhtrang120296@gmail.com
  display_name: Tranglua
  first_name: Trang
  last_name: Le Quynh
---

Chào các bác, 

Hôm nay tôi xin chia sẻ tới các bác về một công cụ hỗ trợ tạo mock server có 
tên là Mountebank.

------------
Tóm tắt
- Contract testing
- Mountebank mock server
- Cách cài đặt
------------

### Contract testing

Đã bao giờ các bác phải sửa đi sửa lại sản phẩm của mình khá nhiều lần vì 
không thể thống nhất được với khách hàng về bộ API các bác đưa ra chưa nhỉ? 
Chắc điều này không còn quá xa lạ với anh em phát triển phần mềm rồi. Để khắc 
phục vấn đề này, tôi sử dụng Contract testing, từ đó việc thống nhất được bộ 
API giữa consumer và nhóm phát triển đã giảm thiểu khá nhiều khó khăn trong 
quá trình trao đổi với consumer và phát triển sản phẩm. Những nội dung đã được 
thống nhất này sẽ căn cứ dựa trên một contract, bao gồm tất cả các thông tin
của API như request, response thế nào, tham số trả về, format của data ra sao,
... Tuy nhiên, một bản contract là chưa đủ, tôi với các bác đương nhiên sẽ 
không thể check bằng cơm cái contract này rồi. Phải có một công cụ hỗ trợ test 
chứ nhỉ ^_^!


### Mountebank mock server

Để áp dụng contract đã thoả thuận vào quá trình phát triển và kiểm tra trải 
nghiệm của khách hàng, một mock server sẽ được dựng lên và Mountebank là công
cụ giúp chúng ta làm điều đó hết sức dễ dàng. 

Mountebank là một công cụ open source cho phép test  
Mountebank mock server sẽ trả về 
response khi có request từ người dùng gửi tới. Các response và request này sẽ 
do chúng ta định nghĩa dựa trên contract đã có. Từ phía consumer, Mountebank
mock server

