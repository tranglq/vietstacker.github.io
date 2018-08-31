Dear các ace,

Ta thấy sự phát triển một cách mạnh mẽ khái niệm về edge cloud, đặc biệt là trong telco. Tuy nhiên, tại các edge cloud, ko chỉ đơn thuần có các network slides, các app đặc thù của telco, mà còn một vấn đề khác cần phải quan tâm, đặc biệt là các khách hàng của các telco.

Vấn đề đặt ra ở đây chính là: Operators sẽ có thể chạy các 3rd party applications cùng với các network functions trên cùng các telco edge sites? Giả thuyết được đặt ra ở đây chính là khối app này dưới góc nhìn của khách hàng của các doanh nghiệp telco sẽ có thể như sau:

- Như là một single edge cloud
- Một phần của khái niệm multi cloud
- Chạy như là một single env trên mỗi site edge cloud
- Chạy như là một multi-enviroment trên mỗi site edge cloud

a. Dưới góc nhìn của Multi-cloud: 

Dứng dưới khái niệm multi-cloud concept thì ta nhận thấy rõ rệt thế này, bên cạnh việc sử dụng giải pháp về multi-cloud là cần thiết cho redundancy cũng như vendor lock-in thì multi-cloud cũng chính là một giải pháp cho các vấn đề liên quan đến mở rộng kinh doanh cũng như các mục tiêu về kỹ thuật. Các mục tiêu này bao gồm như lợi dụng các lợi thế kỹ thuật của các cloud provider tại một vị trí (địa lý) nào đó. Cũng như vậy, đối với các telco edge cloud thì việc tân dụng lợi thế của các telco app mang lại tại một vị trí địa lý nào đó nên các 3rd party apps có thể được deploy ngay tại telco edge cloud. Tất nhiên việc deploy 3rd app ko phải là deploy toàn bộ app đó tại edge cloud mà có thể một phần nào đó tại edge, một phần khác thì có thể tại AWS, Azure, etc.

b. Dưới góc nhìn Single/Multi-env tại một single telco edge site

Điều này có thể xảy ra và phụ thuộc khá nhiều vào nhu cầu của thị trường. Một trong những ví dụ chính là việc sử dụng một single env, như PaaS layer. PaaS layer này có thể accross thông qua rất nhiều env (edge cloud, AWS, Azure, etc.). Nói một cách nôm na, ta có thể dùng duy nhất một PaaS nào đó mà nó có thể được dùng để deploy các VNF, cũng như PaaS này có thể kết nối đến các dịch vụ cloud của các provider khác, như Azure, AWS, etc. Điều đó có nghĩa là enterprise can thể có một distributed app.

Một trong những góc nhìn khác chính là việc sử dụng multi-env tại cùng một telco edge. Tất nhiên, ta sẽ dùng một env cho các VNF, các env còn lại sẽ có thể là AWS greengrass, Azure IoT, PaaS layerj, etc. cho các ứng dụng khác.

Chính vì nhu cầu lớn của edge cloud theo nhu cầu của thị trường, cho nên các hãng cũng phải chuyển đổi theo để bắt kịp thị trường, chính là việc hợp tác với nhau. Ví dụ như: PKS (Pivotal Container Platform) chính là một sản phẩm được tạo ra thông qua sự hợp tác giữa VMWare, Pivotal và Google. Do đó PKS có thể chạy trên VMWare cũng như Google Cloud Platform.

To be continued....
