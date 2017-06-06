---
layout: post
title: Rolling-Upgrade [Part2]
date: 2017-06-05
type: post
published: true
status: publish
categories:
- Bài dịch - Tài liệu
- Chia sẻ kinh nghiệm
- Hướng dẫn - Kinh Nghiệm
- Tài liệu tham khảo
tags:
- Rolling-Upgrade
- Microversion
meta:
  _publicize_pending: '1'
author:
  login: Nam
  email: namptit307@gmail.com
  display_name: namnh
  first_name: Nam
  last_name: Nguyen-Hoai
---
## Rolling-Upgrade [Part 2]

Trong phần tiếp theo của chủ đề **Rolling upgrade** tôi sẽ phân tích về tám tính năng [1] mà một project cần được hỗ trợ để được công nhận có tính năng Rolling upgrade.

--------------------------------

### 1. Online Schema Migration
Cho phép thay đổi lược đồ cơ sở dữ liệu mà **không** yêu cầu tắt hết dịch vụ (không hỗ trợ thay đổi về phiên bản cũ hơn).
Lược đồ cơ sở dữ liệu ở đây ta có thể hiểu là database của một service. Hãy thử hình dung rằng trong phiên bản kế tiếp database đã bị thay đổi như thêm cột/bảng, xóa cột/bảng, hoặc sửa lại tên cột/bảng. Vậy làm thế nào mà ta có thể upgrade database lên phiên bản kế tiếp mà không phải tắt hết tất service cùng một lúc rồi bật, đúng là một bài toán khó đúng không các bạn đọc :) Và có một lời giải cho bài toán này là sử dụng tính năng `trigger` trong database và cụ thể trong Openstack đã có Keystone và Glance sử dụng tính năng này để thực hiện việc **Rolling upgrade**.

Vậy `trigger` là gì: Trigger được hiểu đơn giản là một thủ tục được thực thi trong database khi có một sự kiện xảy ra như update, insert hay delete.
*Ví dụ*: ta có thể tạo ra một trigger để trước khi ghi một bản ghi vào trong table A thì tạo một bản ghi vào trong table B.

Bây giờ tôi sẽ lấy một đầu bài và xin các bạn hãy  **luôn nhớ** nó trong suốt quá trình đọc bài này để có thể hiểu xuyên suốt nhé.

**Bài toán:** Database của một service tại phiên bản N có một bảng tên là "old" nhưng sang database tại phiên bản N+1 đã bị xóa và thay thế bằng bảng mới tên là "new". Vậy làm thế nào để người dùng service đó tại phiên bản N có thể *rolling upgrade* lên phiên bản (N+1).

Sử dụng trigger và một trong những lời giải cho bài toán này và tôi sẽ phân tích từng giai đoạn để các bạn có thể thấy được không có thời điểm nào có service xung đột với database.

Trong quá trình rolling upgrade database sử dụng trigger ta sẽ có 3 pha chính:
- Expand: dùng để thêm cột, bảng, trigger trong database.
- Migrate: dùng để di chuyển dữ liệu từ cũ sang mới.
- Contract: dùng để xóa cột, bảng, trigger trong database.

Hãy thử hình dung rằng có hai service (A và B) cùng tương tác với database:
![image1](../pictures/two_services.png)

Và quá trình rolling upgrade sử dụng trigger có các pha như sau:
![image2](../pictures/rolling_upgrade_part_2.png)

- Pha (1): Chuẩn bị upgrade

Khi hai service (A và B) đang chạy tại phiên bản N thì chúng đều tương tác với "old" table.

- Pha (2): Expand database

Lúc này database sẽ có "new" table và trigger. Nếu trên A và B tạo ra các bản ghi ở "old" table thì trigger sẽ làm nhiệm vụ copy nội dung cần thiết của bản ghi đó sang "new" table.

Hình ngũ giác màu da cam thể hiện các dữ liệu được tạo ra tại pha expand, và hình tam giác màu da cam thể hiện các dữ liệu được tạo bởi trigger.

Hình ngũ giác màu xanh da trời thể hiện các dữ liệu đã có sẵn trước pha expand.

- Pha (3) Migrate database

Copy toàn bộ dữ liệu ở "old" table trước thời điểm tạo trigger sang "new" table. Nghĩa là tất cả các hình ngũ giác màu xanh da trời sẽ được copy sang thành hình tam giác tương ứng tại "new" table.

- Pha (4) Deploy phase

Nhờ có tính năng rolling upgrade mà ta không phải upgrade hai service cùng một thời điểm mà ta có thể làm lần lượt:

*Thời điểm 1:* Upgrade service A lên phiên bản (N+1):
A sẽ đọc ghi vào "new" table, và khi tạo ra một bản ghi mới ở "new" table thì cũng sẽ được trigger copy sang "old" table.
Đối với B thì vẫn đọc tại "old" table và nếu ghi vào "old" table thì cũng được trigger copy sang "new" table.

*Thời điểm 2:* Upgrade service B lên phiên bản (N+1):
Lúc này tất cả các serivice đã được upgrade lên (N+1)

- Pha (5) Contract phase

Sau khi đã upgrade toàn bộ serivce lên (N+1) thì "old" table sẽ không được đọc ghi bởi bất kì service nào nữa, tất cả đã chuyển sang đọc ghi "new" table
Thực hiện contract phase sẽ xóa trigger và xóa "old" table

- Pha (6) Hoàn thành

Chúng ta đã hoàn thành quá trình upgrade hệ thống sử dụng trigger để xử lý vấn đề thay đổi database. Trong toàn bộ quá trình, service tại phiên bản N hay (N+1) cũng đều có thể tương tác với database. Do vậy sẽ giảm được downtime hệ thống khi upgrade.

Tiếp theo tôi sẽ phân tích tính năng thứ hai.

### 2. Maintenance Mode
Là chế độ đặt một node vào trạng thái duy trì, bảo hành sửa chữa để cho hệ thống không đặt resources trên node đó nữa khi có yêu cầu từ người dùng tạo mới resources. Tôi sẽ lấy một ví dụ cụ thể để mà một project đã tích hợp tính năng này vào đó chính là Nova.

Trong Nova có các thành phần như: nova-api, nova-conductor, nova-scheduler, nova-compute,...

Trong đó:
- nova-compute dùng để tạo ra các máy ảo, vậy máy ảo ở đây chính là resource.
- nova-scheduler sẽ làm nhiệm vụ tính toán để đặt con máy ảo trên nova-compute nào cho phù hợp.

Vậy câu chuyện là khi người vận hành muốn bảo trì một node nova-compute 30 thì đầu tiên họ phải ra lệnh cho nova kích hoạt chế độ **maintenance mode** cho node nova-compute 30 lên, lúc đó nova-scheduler sẽ không đặt máy ảo trên node nova-compute 30 nữa kể cả tài nguyên trên đó vẫn còn rất nhiều khi có yêu cầu từ người dụng tạo máy ảo. Để người vận hành có thể thao tác thoải mái với node nova-compute 30.
Đó chính là ý nghĩa của tính năng **Maintenance Mode**.

Trong bài tiếp theo, tôi sẽ tiếp tục phân tích về các tính chất còn lại của tám tính chất [1] trong Rolling upgrade.

Thân ái, hẹn gặp lại!
Nguyễn Hoài Nam

[1] https://specs.openstack.org/openstack/openstack-user-stories/user-stories/proposed/rolling-upgrades.html#gaps
