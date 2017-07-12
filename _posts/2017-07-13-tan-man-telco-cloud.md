---
layout: post
title: "tan-man-telco-cloud"
date: 2017-07-13 01:11:18
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
---

Hôm nay sau khi meetup OPNFV tại Budapest - nội dung chỉ đơn thuần là cập nhật những tin mới từ summit 2017 - tôi có dịp được gặp manager về cloud của Ericsson. Cá nhân tôi cũng không nghĩ lại được gặp ông ấy tại buổi meetup này và càng bất ngờ hơn khi ông ấy chia sẻ rất nhiều những tư tưởng rất hay. Sau một vài phút trao đổi về rất nhiều chủ đề, từ buzz word như devops cho đến việc các telco bây giờ phải làm gì trong thời buổi cloud này, chiến lược ra sao, tôi xin phép được tản mạn một chút dưới đây nhé:

1.Có một hiện trạng đang diễn ra như thế này, các CEO, các big boss họ đều nhìn ra được vấn đề hiện tại của chính tập đoàn của mình. Họ biết rằng tập đoàn phải thay đổi theo hướng A, B, etc. để phù hợp với xu thế, tuy nhiên vấn đề lại nằm ở các managers "ở giữa", các cầu nối từ big boss đến các kỹ sư. Với cấu trúc rất lằng nhằng của các tập đoàn lớn thì sự phân cấp vô cùng lớn, rất nhiều cấp bậc. Thực tế chỉ ra rằng, với sự phát triển một cách vũ bão về công nghệ hiện nay, thì chính các "cầu nối" này lại không theo kịp. Họ không hiểu được bản chất của cái dự án, nhỏ hơn là các tính năng mà các team của mình đang làm, nó dùng để làm gì, tại sao lại phải có nó, nó phù hợp với định hướng nào, etc. Chính vì vậy, mặc dù các big boss họ đưa ra một lộ trình rất cụ thể, nhưng đến các middle managers thì họ lại mù mờ trong định hướng, họ ko biết phải làm gì, cụ thể hóa nó như thế nào, dẫn đến việc lead team đi chệch hướng :(. Do đó, khá nhiều các quan điểm cho rằng, có hay chăng ta nên cắt giảm các "cầu nối" để tư tưởng và định hướng đến gần hơn với các kỹ sư?

2.Một trong những buzz word gây nhiều tranh cãi chính là "devops", chúng tôi trao đổi khá sôi nổi xung quanh cái này. Ericsson hiện nay đã giảm một loạt các devops mà thay vào đó chuyển sang làm operators. Vì sao ư? Đơn giản vì mấy cái sản phẩm mã nguồn mở trên cloud này nó quá ư lằng nhằng khiến cho nếu tiếp diễn tình trạng devops thì sẽ ko đáp ứng được yêu cầu khách hàng đúng thời hạn. Ta có thể nhận thấy rằng, các sản phẩm nguồn mở xoay quanh cloud hiện nay nó vừa phải hỗ trợ chạy trên nhiều distros, mở rộng ra thì ta có cụm từ "infrastructure agnostic", nó cần phải được deploy/upgrade/scale/heal, etc. một cách tự động, chưa kể sự phức tạp trong source code. Nó vô cùng khác với cái việc phát triển sản phẩm, đóng gói và install trên hardware và bán nó như là một appliance. Chính vì vậy, cái mà khách hàng muốn trước tiên nhất là "MAKE IT WORK". Tất nhiên ta ko xét đến những ông lắm tiền nhiều của như Huawei hay China Mobile, chúng xua quân vào mọi dự án một cách vô tội vạ :D.

3.Vậy trong telco, chiến lược của các đại gia như Ericsson hay Nokia là gì trong sự phát triển quá nhanh của cloud hiện nay? Sự phát triển của Internet, 5G, các phương tiện truyền tải thông tin thông qua Internet như Viber, Skype, Webex, etc. giáng một đòn mạnh vào mạng radio truyền thống. Giờ chỉ cần có internet, ta hoàn toàn có thể liên lạc với người khác mà ko cần radio mobile. Tất nhiên, sự truyền tài thông tin thông qua sóng radio vẫn rất cần thiết, ví dụ như những nơi mà internet không tới được hoặc trong một số ngành nghề đặc thù hay ta cần phải dùng radio thông qua vệ tinh. Nhưng sự thật chỉ ra rằng, thị trường của việc truyền đạt thông tin thông qua sóng radio sẽ ngày một hạn hẹp lại, sân chơi dành cho internet. CEO của Ericsson có chỉ ra rằng, trong các năm sắp tới thì 30% thị trường sẽ là sóng radio, 70% sẽ là việc truyền tải dữ liệu thông qua internet, đơn giản là vì đây là thời đại của dữ liệu (data), mọi thứ đều là dữ liệu. Chính vì vậy với các telco lớn thì việc chuyển dịch này mang tính quyết định. Họ không thể mãi là một network provider được, họ phải chiếm lĩnh thị trường cloud, cộng với thế mạnh tuyệt đối về network để cung cấp các giải pháp về data thông qua internet. Cụ thể hơn là họ muốn xâm nhập thị trường IT, trở thành một IT provider cung cấp các giải pháp cloud cho các tòa nhà, ngành nghề, các thành phố, quốc gia. Các bạn có thể tưởng tượng thế này, các telco sẽ bán các sản phẩm dựa trên nền tảng cloud để các khách hàng truyền các dữ liệu thông qua internet lên trên cloud đó. Tại đây, dữ liệu sẽ được xử lý, phân tích, đánh giá và trả về cho khách hàng. Đây cũng chính là chiến lược mà Ericsson hay Nokia đang hướng tới, chỉ có điều họ có đủ nhanh để chuyển mình hay không, ta hãy chờ xem nhé :)


Tutj/VietStack


