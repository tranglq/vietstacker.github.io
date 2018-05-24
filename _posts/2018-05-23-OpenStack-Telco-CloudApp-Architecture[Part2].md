Ở phần 1, ta đã có cái nhìn về sự phát triển của cloud dưới góc độ "stack", hi vọng các bạn đã có cái nhìn tổng quát nhất. Vậy trong một stack, ta sẽ bóc tách thành các phần nhỏ để có cái nhìn rõ ràng hơn.

Thành phần đầu tiên tôi muốn đề cập đến, đó chính là SDDC (Software Define Data Center). Đây chính là phần hạ tầng cho toàn bộ các VNF hay Cloud App phía trên. Trước đây ta chỉ đề cập đến VIM (Virtual Infra Manager) nhưng VIM là khái niệm của ETSI, về mặt bản chất, nếu ta mở rộng VIM dưới góc độ là manager của nhiều DC thì VIM cũng chính là SDDC. Nhiệm vụ của SDDC chính là lớp manager của tầng infra cho nhiều loại DC, bao gồm Local DC, Edge DC, Centralized DC, thậm chí cả pubic cloud nếu ta triển khai Hybrid. Tuy nhiên SDDC cần có cái gì? Ta bóc tách nhỏ hơn nữa thì nó sẽ thế này:

SDA (Software Defined Accelerators)
SDN (Software Defined Network)
SDC (Software Defined Computing)
SDS (Software Defined Storage)
SDF (Software Defined Facilities)
SDSec (Software Defined Security)

Các bạn có thể thắc mắc là, tại sao lại phải có ông accelerator hay sec hay facility ở đây. Vì đơn giản là trong quy mô của thiết kế tổng thể, tôi muốn nhắc đến ý niệm: Infra Agnostic. Tức là các Telco App sau này cũng như Cloud App phải có khả năng chạy trên bất cứ Infra nào cũng như một Infra có thể được dùng cho bất cứ app nào. Tuy nhiên trong thực tế thì tùy từng app ta sẽ có yêu cầu Infra cụ thể hơn. Trong bài viết này, tôi sẽ tổng hợp chung các yêu cầu cho một Infra Agnostic.

Ta sẽ lần lượt đảo qua các yêu cầu cần thiết cho từng thành phần, đầu tiền là SDA. Rõ ràng ta nhận thấy được rằng, trong các DC, để tăng performance thì việc ứng dụng các accelerators là không thể tránh khỏi. Một vài các ứng cử viên về hardware nặng kí như: FPGA, GPU, ASIC. Vậy, các accelerator services bao gồm những gì? Tôi xin tổng hợp một số yêu cầu quan trọng như sau:

- Encryption
- Hashing
- RNG (Random Number Generator)
- Packet Dispatch
- Compression
- Transcoding
- Digital Signal Processing
- AI
- Analytics

Nói tóm lại, các accelerator services này sẽ được điều khiển (control) bởi software để tăng sự linh hoạt. Việc điều khiển này được thực thi thông qua các API. Do đó, trong thực tế thì FPGA khá OK cho việc cung cấp các API để giao tiếp.

To be continued 
