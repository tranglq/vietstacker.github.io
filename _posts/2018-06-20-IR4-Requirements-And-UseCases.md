Như ta đã biết thì IR4 sẽ bao trùm lên các công nghệ mới nhất hiện nay: Cloud, Big Data, AI, etc. tuy nhiên, trong khi các công ty, tập đoàn đã và đang phát triển từng mảng riêng lẻ theo từng business riêng biệt thì một câu hỏi đặt ra là: Giả sử các công nghệ trên đều đạt những thành tựu nhất định với các sản phẩm rõ rệt, ta phải kết nối các sản phẩm đó như thế nào để tạo thành một hệ sinh thái IR4? 

Nếu ta trả lời tốt câu hỏi này thì đây sẽ là một định hướng phát triển vô cùng quan trọng với một tầm nhìn dài hạn. Trong phạm vi bài viết này, chúng ta sẽ cùng bàn luận về câu hỏi trên.

Cá nhân tôi nghĩ, nếu ta nhìn các sản phẩm công nghệ cao hiện nay là những ứng dụng, những platforms thì cái ta cần chính là một underlaying infrastructure - cái cho phép ta tạo nên môi trường cho toàn bộ hệ sinh thái của IR4. Ta cần phải có một cái nhìn tổng thể về sự kết nối của các chức năng, thành phần của underlaying infrastructure aka "Industrial Communication Infra" - một Infra cho phép các công nghệ được chấp nhận bởi các ngành công nghiệp trong tương lai hoạt động một cách linh hoạt và trơn tru. Infra này sẽ cho phép các platforms và các ứng dụng sau này tập trung vào các business đặc thù của các ngành công nghiệp. 

Vậy, infra này được cấu thành như thế nào? Quan điểm cá nhân của tôi, nó sẽ phải chứa 3 khu vực công nghệ sau:

- Operational Technology (Safety, Efficiency, Monitoring, etc.)
- Information Technology (Automation Deployment, Fast Data, Edge Cloud, etc.)
- Communication Technologies (Mobility, Network Security, 5G, etc.)

và cụ thể hơn, dưới góc nhìn kiến trúc, nó phải bao gồm 2 layers như sau:

- Một layer cung cấp khả năng truy cập có dây (wired) và không dây (wireless) cho phép các object của một phạm vi nào đó kết nối với nhau và kết nối chúng với các nguồn lực về điện toán đám mây trong những network domain khác nhau. Phạm vi ở đây chính là trong một nhà máy (smart factory), thành phố (smart city), etc. Các network domain khác nhau chính là Private Cloud, Public Cloud hay Hybrid Cloud.

- Layer thứ 2 chính là layer về ứng dụng và data bao gồm các ứng dụng riêng biệt theo từng phạm vi (như trên nhắc đến góc nhìn về phạm vi), data model, data management, data analytics, data visualization cũng như AI hay Machine Learning. Ví dụ đơn giản thế này, khi các sản phẩm công nghệ cao như các IoT devices thu thập các dữ liệu realtime sẽ đẩy về một platform nào đó liên kết với các IoT devices trên, bài toán đặt ra là ta cần phải giải quyết một khối lượng lớn realtime data từ platform kể trên để đưa ra các tác vụ tiếp theo. Layer này sẽ giúp ta làm việc đó.

Tóm gọn lại, ta cần thiết phaỉ xây dựng một underlaying infra bao gồm 2 layer kể trên để phục vụ cho hệ sinh thái IR4. Tuy nhiên, nhìn từ góc độ high-level ta thấy được cấu trúc tổng quan, vậy infra này phải thực hiện được những yêu cầu cụ thể như thế nào từ phía hệ sinh thái IR4 thì ta cần phải định nghĩa được chính những yêu cầu này, từ đó định nghĩa tiếp các use case được sinh ra tiếp theo từ những yêu cầu trên.

Vậy, các yêu cầu đó là gì, các use cases đó là gì, chúng ta sẽ thảo luận tiếp trong part 2 nhé.

To be continued ...

VietStack/Tutj
