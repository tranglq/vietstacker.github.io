---
layout: post
title: DVR in OpenStack
date: 2015-07-18 05:26:25.000000000 +07:00
type: post
published: true
status: publish
categories:
- Tech
- News
tags:
- DVR
meta:
  _wpcom_is_markdown: '1'
  _edit_last: '61498925'
  geo_public: '0'
  _publicize_job_id: '12837946596'
author:
  login: vnstack
  email: vietstack@gmail.com
  display_name: vietstack
  first_name: Viet
  last_name: Stack
---
<p align="justify">DVR là một trong những tính năng khá thú vị và vô cùng cần thiết để giải quyết bài toán về bottleneck cho virtual router của Neutron. Bài viết về DVR của bạn Nguyen Hoai Nam sẽ làm rõ cho chúng ta thấy được những ưu điểm khi sử dụng DVR.</p>
<p align="justify">"<a name="_GoBack"></a><span style="font-family:'Times New Roman', serif;"><span style="font-size:medium;">Đối với mô hình multi node thuần túy việc tạo ra tenant và Router để nối mạng trong ra mạng ngoài khi đó tất cả Router được tạo ra sẽ nằm trên node Network, và khi traffic từ VM đi ra mạng ngoài hoặc VM giữa các tenant với nhau sẽ phải đi qua Router nằm trên node Network.</span></span></p>
<p align="justify"><span style="font-family:'Times New Roman', serif;"><span style="font-size:medium;">Bài toán đặt ra khi có nhiều VM trên hệ thống thì tất cả các traffic của VM đều phải đi vào Router trên node Netowork, lúc đó sẽ gây hiện tượng thắt cổ chai và node Network cũng phải xử lý rất nhiều các traffic của các VM trong hệ thống, để giải quyết bài toán này project Neutron từ bản Juno có thêm tính năng DVR ( Distributed Virtual Router). Đó là tính năng thay vì tập chung Router trên node Network thì Router sẽ được phân bố ra các node Compute để các Router đó xử lý luồng traffic của những VM nằm chính trên node Compute đó. "</span></span></p>
<p align="justify">
<p align="justify">Link attach file:</p>
<p align="justify"><a href="https://vietstack.files.wordpress.com/2015/07/dvr-in-neutron_final.docx">DVR in Neutron_final</a></p>
<p align="justify">
<p align="justify">
<p align="justify">18/7/2015</p>
<p align="justify">VietStack Team</p>
