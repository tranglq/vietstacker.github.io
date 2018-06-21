Tiếp theo part 1...

Câu chuyên giờ bắt đầu với việc ta định nghĩa các yêu cầu của cái Infra như ở phần 1 như thế nào, nó phải hỗ trợ được gì cho toàn bộ hệ sinh thái IR4 cũng như các use cases của Infra này như nào? Việc định nghĩa các use cases được định hình thông qua các tiêu chuẩn cơ bản như sau:

- Nhu cầu/đòi hỏi của các ngành công nghiệp.
- Các yêu cầu rõ ràng về các công nghệ mới.
- Mang đến các cơ hội mới về business.

Từ các tiêu chuẩn như trên thì Infra này phải hoàn thành cũng như đáp ứng được các đòi hỏi như sau:

- Tất cả các object trong một phạm vi ứng dụng được kết nối. Phạm vi ở đây là factory, city, etc.
- Tất cả các object phải trở nên "di động" (mobile). Điều đó đồng nghĩa với việc toàn bộ phạm vi ứng dụng phải dùng mạng không dây.
- Ứng dụng trí thông minh nhân tạo vào trong sản xuất
- Giao tiếp nhanh, ổn định giữa các máy móc và các tiến trình điều khiển.
- Việc triển khai/hoạt động các ứng dụng và platform phải tự động hóa.
- Việc giao tiếp và xử lý dữ liệu trong các tiến trình và phạm vi công nghiệp phải secure, scalable và comprehensive.


Đó, vậy là ta thấy được kiến trúc của Infra này dưới góc độ kỹ thuật là nó phải bao hàm được 2 layers chính nhìn từ high-level và bản thân nó phải giải quyết được các đòi hỏi phía trên.

Việc tiếp theo là ta cần phải định nghĩa các use cases cụ thể trong IR4 và làm sao thực thi các use case đó mà vẫn phải đảm bảo được các tiêu chí trên. Ngoài ra, ta cũng cần phải xem xét việc ứng dụng các công nghệ nào trong công cuộc này. Một số công nghệ tất yêu bắt buộc phải có: Cloud computing, Virtualization and Edge Computing, 5G, BigData/Data Analytics, AI.

Vậy use case là gì? Câu hỏi này nghe chừng đơn giản nhưng cũng ko hề dễ trả lời :D. Thực sự là use case nó vô vàn lắm và phải phụ thuộc vào từng khu vực/phạm vi ứng dụng IR4 cụ thể thì mới có thể định nghĩa được. Sau đây mình sẽ chỉ trình bày qua về việc định nghiã các use cases trong phạm vi của Smart Factory, những phạm vi khác các bạn có thể lấy phạm vi là Factory để mở rộng ra. Cá nhân mình sau khi tham khảo một số dự án đã làm, nghe ngóng được một số thông tin hay cũng như được sự chỉ bảo của một số chuyên gia nơi mình làm việc thì mình sẽ chia nó thành 4 use case cluster:

- Value Chain Integration : Bao gồm các quá trình được optimized và các business model mới được sản sinh thông qua một chuỗi các giá trị công nghiệp (industrial value chain). Một trong những use case cụ thể trong cluster này chính là Automated Guided Vehicles. Ta tưởng tượng rằng, sau này các vehicles trong một nhà máy, thành phố tự động làm việc dựa theo một chuỗi các lênh liên tiếp nhau. Để làm được việc này thì Data Handling cũng như AI trong Data Handling sẽ cần phải được ứng dụng tối đa.

- Production Information Transparency: Tập trung vào "digital twin" (các bạn tự search google nếu ko biết nhé :D) của các process cũng như conditions trong một phạm vi, đặc biệt là trong sản xuất. Một trong những use case cụ thể chính là Remote Machine Access, tức là việc transfer một đống các dữ liệu real-time giữa các địa điểm hoặc phạm vi khác nhau.

- Augmented Worker: Cái tên nói lên tất cả :D. Hỗ trợ con người như là một worker trong sản xuất với cái hệ thống Augmented. Các bạn có thể lấy Travis trong bộ phim IronMan là một ví dụ.

- Versatile Production: THích ứng với việc sản xuất các sản phẩm dựa theo tiêu chí "lot size of one". Một trong những công nghệ hay ho được ứng dụng vào đây chính là 3D printing.


Phần tiếp theo ta sẽ nói qua về các tiêu chí/đòi hỏi phía trên nhé.

To be continued...
VietStack/Tutj
