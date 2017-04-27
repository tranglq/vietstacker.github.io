---
layout: post
title: Buzzword-Serverless-And-Devops
date: 2017-04-27
type: post
published: true
status: publish
categories:
- Chia sẻ kinh nghiệm
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

Có quá nhiều lý thuyết, khái niệm liên quan đến 2 từ này: Serverless và Devops. Và tất nhiên tôi cũng sẽ không nhắc lại nữa, tôi sẽ chỉ tản mạn xung quanh 2 từ này để các bạn có thể thấy được trong thực tế nó như thế nào.

### Serverless

Nói nôm na là "không server". Cái vấn đề là không server ở đây là server nào, đó là server (vật lý, ảo hóa) nơi mà các app chạy trên? Để trả lời câu hỏi này thì tôi nghĩ hay nhất là ta nên đứng dưới góc nhìn nào để định nghĩa nó. Và cá nhân tôi nghĩ thì hay hơn cả là ta nên đứng dưới cái nhìn của app/người deloy app. Thêm một lưu ý nữa tức là nói đến serverless thì ta nên nói trong bối cảnh cloud.

Cụ thể thế này, nếu tôi có cái app A, bây giờ cái tôi cần là tôi không muốn bận tâm đến database, đến HA, đến các frameworks hỗ trợ, tôi muốn cái app của tôi nó chạy trên cloud mà tôi không phải động đến những cái thứ khác. Cái tôi cần quan tâm duy nhất chính là business logic trong cái app của tôi. Từ bài toán này thì ta có thể nhận thấy được là, “serverless” ở đây nó không ám chỉ riêng cái từ “server”, mà nó ám chỉ toàn bộ phần background (infra và platform). Cho nên để support cho khái niệm “serverless” thì toàn bộ phần background ở trên cần phải được “cloud” hóa.

Tuy nhiên, nếu các bạn có mononith app hoặc legacy app thì các bạn sẽ gặp rất nhiêu rắc rối trong việc đưa nó lên cloud với tiêu chí serverless. Vì với các app ngày xưa thì các bạn phải cần có khá nhiều các thành phần, e.g. API server, các libraries, etc. tuy nhiên với serverless, những thứ đó sẽ được cloud support, việc còn lại là thiết kế các functions để thể hiện business logic. Tập hợp các function sẽ tạo lên app. 

Qua đó ta thấy rằng, với serverless, các app sẽ chỉ còn là tập hợp các function. Ta có thể upload các functions này lên cloud và chỉ định datacenter mà ta muốn nó chạy trên, e.g. DC Bắc Mỹ chẳng hạn:d. Ngoài ra ta sẽ phải chọn xem ta muốn RAM là bao nhiều, disk dung lượng bao nhiêu, cao cấp hơn thì QoS là bao nhiêu, etc. và ta sẽ phải trả tiền cho số dung lượng ta dùng. Một ví dụ cụ thể hiện nay chính là AWS Lambda của Amazon. Cá nhân mình không cổ xúy cho bất cứ giải pháp nào nhưng phải công nhận một điều, Amazon thực sự mạnh, rất mạnh là đằng khác.

### Devops

Nói về ông này thì nhiều vô kể nhưng tôi muốn nhấn mạnh đến một khía cạnh sau của nó mà rất nhiều người lầm tưởng: Docker/Container sẽ thay thế/giết Devops

Cái anh Devops ra đời với một trong những sứ mạng quan trọng: Automate mọi thứ. Tức là thế này các bạn ạ. Các app được cấu tạo bởi rất nhiều phần, mỗi phần dùng nhiều thư viện khác nhau, rồi phần này liên quan đến phần kia, tác động đến phần khác. Chưa kể đến quá trình packaging mà tất nhiên là các ngôn ngữ lập trình sẽ chả giúp ích được cho ta trong tiến trình này. Do đó ta mới đẻ ra cái CI, CD. Lúc này các commit sẽ được đẩy lên CI, CI test nó → green → create package → create iso (nếu cần). 

Nói là như vậy nhưng cả đống quá trình trên vẫn khá phức tạp như mà CI sẽ bị break liên tục vì rất nhiều lỗi, một trong số đó là do các dependency. Lúc này thì vị cứu tinh xuất hiện, container technology với đại diện ưu tú: Docker. Khi ta tạo dockerimage thì nó sẽ chứa toàn bộ các libraries trong đó, các component cứ thế mà dùng mà chả phải lo conflict với ông component khác nào. Tập hợp các component chạy trên docker như vậy sẽ tạo thành 1 app.

Và thế là người ta nghĩ, docker sẽ giải quyết các vấn đề trong Devops cho bạn. Nhưng họ đã sai lầm hoàn toàn. Các bạn hoàn toàn có thể dùng CI để test các commit, xong tạo dockerimage rồi launch docker. Về phần Dev thì không vấn đề gì nhưng ai sẽ maintain nó khi mà biết đâu bất ngờ một ngày đẹp trời, app của bạn gặp lỗi vì một lý do nào đó (ví dụ: Queuing bị lỗi chẳng hạn). Không ai dám khẳng định rằng các bài test trong CI sẽ test toàn bộ các case study có thể gặp trong thực tế, do đó lỗi xảy ra là điều hoàn toàn có thể. Đến lúc này các ops phải xem là lỗi ở docker nào, lấy log gửi về cho dev. Dev ngắm nghía log, đùng cái phát hiện ra bug là do logic trong code. Thế là quá trình từ commit để ra docker lại lặp lại.

Và cái quá trình lặp lại này nó sẽ dễ dàng dẫn đến kết cục như bài viết lần trước trong topic: Devops chuyện giờ mới kể. :D

Nói tóm lại Docker có thể sẽ giúp ta hạn chế phần nào các vấn đề liên quan đến Devops nhưng nó không phải là giải pháp. Đến thời điểm này, không có bất cứ một tool nào có thể giải quyết các vấn đề liên quan đến Devops. Giải pháp duy nhất chính là: CON NGƯỜI.

Sự tương tác giữa người và người (dev và ops) là giải pháp duy nhất. Do đó, với rất nhiều công ty, ngoài kỹ năng chuyên môn ra thì họ rất khắt khe với cái chuyện: khả năng tương tác với đồng nghiệp như thế nào. Đôi lúc chuyện này còn quan trọng hơn cả chuyên môn đấy :D.


27/4/2017

VietStack team
