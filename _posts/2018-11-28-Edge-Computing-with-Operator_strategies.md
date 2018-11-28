Trong khoảng 3-5 năm trở gần đây, edge computing và hệ sinh thái của nó đã ngày càng một phổ biến với rất nhiều dự án, rất nhiều use cases. Bên cạnh đó, các operators cũng đưa ra rất nhiều các hoạch định chiến lược về việc chiếm lĩnh thị trường của các dịch vụ liên quan đến edge computing, nhưng đối với mình thì quả thực nhiều lúc khá là bối rối vì có quá nhiều projects sinh ra mà không thực sự rõ ràng là dùng nó để làm gì, use cases như thế nào, nếu dùng nó thì mang lại lợi ích gì, etc. vì rốt cuộc lại là đối với các operators thì chiến lược gì thì chiến lược, quan trọng nhất cũng phải sinh lợi nhuận.

Dạo qua một vòng thị trường về các dự án, sản phẩm liên quan đến edge computing thì ta có một loạt cái tên nổi bật như Akraino, EdgeX Foundry, ETSI MEC, OpenFog Consortium, Rafay Systems, Packet, Telstra, etc. Các thông tin của các dự án ta có thể dễ dàng google, tuy nhiên với cá nhân mình thì với một đống các dự án đẻ ra như thế này, rốt cuộc các operators định làm gì, định chiếm lĩnh thị trường gì, cụ thể hình dung nó như thế nào? Với mình thì nó có thể được nhìn nhận dưới 3 góc nhìn cũng như 3 hướng đi của các operator như sau:

- Deploy/manage/own các edge location.
- Connects/inter-manages các edge nodes/locations.
- Enable các edge-based applications.


1. Deploy/manage/own các edge location.

Với vai trò này, các operators sẽ triển khai, quản trị và trở thành "người sở hữu" các edge locations. Họ sẽ trở thành các nhà cung cấp các edge locations cho các tập khách hàng muốn sử dụng các location này để mở rộng các dịch vụ cloud của họ. Các edge location sẽ cung cấp các nguồn lực về cloud tại edge như computing, servers, fibre connectivity, etc cho các 3rd parties. Ngoài việc có thể họ sẽ phải setup các edge locations mới (việc này tốn khá nhiều chi phí) thì họ sẽ tận dụng các cơ sơ vật chất có sẵn về hạ tầng như các tòa nhà hay các văn phòng hiện có của các operators. Ví dụ như Telefonica hay Telstra vừa có kế hoạch biến hàng nghìn văn phòng của họ thành các edge data center.

Tuy nhiên, thị trường cho hướng đi này khá là "bị giới hạn". Hiện tại các công ty sẵn sàng sử dụng các edge location kiểu này thì phần lớn là các công ty về video hay CDN. Netflix là một ví dụ điển hình. Do đó, nhu cầu hiện tại cho hướng đi này không nhiều và sự cạnh tranh khá cao như mà Netflix rất sẵn sàng tìm kiếm sự thay thế cho edge locations để đạt hiệu năng cao hơn.

Ngoài ra, một vấn đề nữa chính là việc chọn lựa các kiến trúc edge cho edge locations. Hiện tại thì ETSI MEC đang chiếm phần trăm cao nhất trong việc trở thành kiến trúc chính cho edge location mà các operators mong muốn. Tiếp theo sau chính là OpenFog mặc dù hướng đi của OpenFog hơi khác một chút so với ETSI MEC. Trong khi ETSI MEC hướng tới việc định nghĩa các APIs để giao tiếp với các edge location cũng như hướng tới các location chính xác của edge thì OpenFog tập trung vào việc định nghĩa các tiêu chuẩn để đảm bảo rằng tất cả các edge resource có thể tương tác với nhau, không quan trọng vị trí các edge ở đâu. Tất nhiên việc phân tán các edge resource này sẽ phụ thuộc vào các yêu cầu cụ thể của các apps. Bên cạnh đó, gần đây nổi lên StarlingX - một dự án của OpenStack foundation tuy nhiên nó vẫn còn mới sơ khai.

2. Connects/inter-manages các edge nodes/locations.

Điều này đồng nghĩa với việc các operators phải giải quyết các vấn đề liên quan đến việc kết nối các edge location mà không quan trọng nó ở đâu. Họ cần phải có các giải pháp về SDN để cung cấp một platform cho việc kết nỗi các private edge locations (của một công ty hay doanh nghiệp nào đó) tới các edge locations của các 3rd parties khác cũng như các edge locations của chính operators đó thông qua các operator networks. Ví dụ thế này: SDN sẽ cho phép các edge locations download các VNF templates và sau đó các edge locations sẽ tự deploy các VNFs đó trên các edge. Các VNF này sẽ giúp các edge locations kết nối với nhau một cách an toàn hơn thông qua các operator networks.

Điều này sẽ cho phép tạo ra một mạng lưới rất rộng, dày đặc và dễ dàng scale của các edge locations. Các operators cũng đang hứa hẹn về việc chuyển mình đơn thuần từ hướng đi 1 để chiếm thị trường của hướng đi 2. Bên cạnh đó, họ cũng đang đàm phán để "bắt tay" với các edge location providers khác thay vì chỉ sử dụng các edge location có sẵn của họ. Điều này vô hình chung trở thành một cuộc đua mới giữa các operators trong việc bắt tay với các partners để có các vị trí edge tốt nhất. Đây chính là một hướng đi mới, tạm gọi là "neutral host" vì vị trí các operators trong thị trường này chỉ đơn thuần là "người kết nối". Chủ sở hữu các edge location sẽ chính là các đơn vị non-operator.


Vậy, các operators cần phải làm gì nếu muốn chiến thắng trong hướng đi này: 

- Đơn giản nhất là phải xây dựng một cơ sở IaaS có khả năng scale tốt để hấp dẫn các edge location owners, kéo theo việc hợp tác với nhau. Nói nôm na là giờ nếu các ngân hàng, bênh viện là các edge location, Viettel hay VNPT phải xây dựng một hạ tầng có khả năng scale tốt bao gồm các hệ thống base station dày đặc (trong usecases của 5G), một nền tảng SDN cực tốt để đảm bảo các virtualized connection giữa các edge locations hay một chuỗi các edge locations của chính các operators đó để chứng minh năng lực của chính bản thân. Dự án Akraino giúp việc xây dựng các edge locations trở nên nhanh chóng và dễ dàng. Ngoài ra thì Vapor IO cũng đang hứa hẹn việc buiding các edge cloud nodes tại các cell site.

- Cách nữa là có chiến lược để tiến lên hướng đi thứ 3, cung cấp một portfolio đầy đủ. Lúc này các operators sẽ đóng thêm vài trò là edge app enabler.
