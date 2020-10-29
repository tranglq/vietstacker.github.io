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

Chào mọi người, 

Hôm nay tớ xin chia sẻ về một công cụ hỗ trợ tạo mock server có 
tên là Mountebank.

------------
Tóm tắt
- Contract testing
- Mountebank mock server
- Cách cài đặt
------------

## Contract testing

Đã bao giờ các bạn gặp khó khăn trong việc test các API đã thống nhất được 
với khách hàng về bộ API các bác đưa ra chưa nhỉ? Để thống nhất được bộ 
API giữa consumer và nhóm phát triển, chúng ta thường có một bản contract chứa
tất cả các thoả thuận về API. Việc test kết quả dựa trên bản contract được gọi 
là phương pháp contract testing. 
... Tuy nhiên, một bản contract là chưa đủ. Làm thế nào để có thể trực tiếp test
các cases trong contract, so sánh giữa API bản production và API trên contract đã
thoả thuận? Điều này thật dễ dàng nếu bạn có một mock server cung cấp các API
giả lập đấy ^_^!


## Mountebank mock server

Như đã nói ở phần trước, để có thể thực hiện contract testing, chúng ta sẽ dựng một
mock server chứa các API giả lập. Mountebank sẽ giúp chúng ta làm điều đó hết sức dễ
 dàng. 

Mountebank là một công cụ mã nguồn mở đầu tiên cung cấp tính năng 
cross-platform, multi-protocol test double. Nếu bạn đã từng nghe về test double
sẽ biết rằng, test double tạo ra các đối tượng giả lập trong trường hợp các bài
test của bạn có sự phụ thuộc tới một đối tượng khác. Đó chính là công dụng của
của Mountebank đấy. 

Theo phương thức hoạt động, Mountebank sẽ tạo ra tập hợp rất nhiều các importers,
tạm gọi là một đội quân giả mạo đi. Ai dạo này hay chơi Among us chắc không lạ
lẫm lắm nhỉ :D hihi. Các bài test của bạn sẽ kết nối tới Mountebank mock server,
gọi vào API thông qua giao thức HTTP request. Mountebank sẽ xác nhận request của
bạn có gọi vào một API giả lập nào đó hay không, từ đó trả về kết quả kỳ vọng
đã được cài đặt vào server trược đó.

Mountebank sử dụng nhiều loại imposter khác nhau, mỗi giao thức cung cấp một
dạng gói tin phản hồi cụ thể. 


## Hướng dẫn tạo contract với Mountebank sử dụng docker-compose

#### Cấu trúc thư mục
```
---contracts/
    |----consumers/
    |        |----folder-1/
    |                |----folder-1.1/
    |                |       |----folder-1.1.1/
    |                |----imposters-1.json
    |                |----imposters-2.json
    |----.env 
    |----docker-compose-mountebank.yml
    |----imposters.json
```

- `consumers`: thư mục chứa các imposters cần giả lập API. 
- `docker-compose-mountebank.yml`: kịch bản build Mountebank 
- `imposters.json`: là main imposters 

#### Cài đặt mock server với Moutebank trên local
- File `docker-compose-mountebank.yml`:

```yaml
version: "3"
services:
  mountebank:
    container_name: mountebank
    image: ${IMAGE_REGISTRY_URL}/jkris/mountebank:latest
    volumes:
    - "./consumers:/imposters/consumers/:rw"
    - "./imposters.json:/imposters/imposters.json:rw"
    network_mode: host
    working_dir: /imposters
    environment:
      - TZ=Asia/Ho_Chi_Minh
    command: --configfile ./imposters.json --port ${MOUTNTEBANK_PORT}
    restart: always

```

- File `.env`:
```commandline
IMAGE_REGISTRY_URL=10.60.129.132:8890
MOUTNTEBANK_PORT=8585
```

- File `imposters.json`
```json
{
  "imposters": [
      // Đường dẫn tới mỗi imposter
      <% include ./consumers/folder-1/imposters-1.json%>
  ]
}
```
Nội dung của các imposter mình sẽ nói ở phần sau nhé!

- Để build Mountebank mock server trên local, ta thực hiện lệnh sau:
```commandline
docker-compose -f docker-compose-mountebank.yml up -d 
```
- Truy cập vào http://localhost:8585/ để xem giao diện chính của Mountebank vừa 
cài đặt. 
- Truy cập vào http://localhost:8585/imposters để xem chi tiết các imposters 
đã dựng 

#### Imposter
##### 1. Định nghĩa 

`Imposter` là một thành phần trong Mountebank có vai trò như một server giả lập.
Các thành phần trong imposters bao gồm các thông tin của API như:
- port
- protocol 
- stubs 
- ...

##### 2. Thao tác với imposter 
Tài liệu này hướng dẫn các thao tác tạo/sửa/xóa imposters. 

2.1. Để tạo một imposters ta thực hiện các bước:
- Trong thư mục `consumers` tạo thư mục `folder-1`. 
- Trong thư mục `folder-1`, tạo file json chứa cấu hình imposters `imposter-1.json`. 

```json
{
  "protocol": "http",
  "port": 8383,
  "name": "imposter-1",
  "requests": [],
  "stubs": []
}
```
- Import imposter đã tạo vào main imposter:
    
```json
{
  "imposters": [
      <% include ./consumers/folder-1/imposters-1.json%>,
      <% include ./consumers/folder-1/imposters-2.json%>
  ]
}
```
- Test trên localhost:
    - Đảm bảo local đã cài đặt mountebank 
    - Chạy lệnh `docker restart mountebank`
    - Kiểm tra kết quả tại: http://localhost:8585/imposters

Vậy là chúng ta đã có một Mountebank mock server http://localhost:8383/ chứa 
các imposter chúng ta đã cài đặt.

2.2. Để xóa imposter ta thực hiện như sau:
- Xóa dòng lệnh import file json chứa cấu hình imposter cần xóa 
```json
{
  "imposters": [
        <% include ./consumers/folder-1/imposters-1.json%>,
//      <% include ./consumers/folder-1/imposters-2.json%>, ---> xóa 
  ]
}
```
- Test trên localhost:
    - Đảm bảo local đã cài đặt mountebank 
    - Chạy lệnh `docker restart mountebank`
    - Kiểm tra kết quả tại: http://localhost:8585/imposters
    
2.3 Để sửa imposter ta thực hiện như sau:
- Sửa nội dụng trong imposter 
- Test trên localhost:
    - Đảm bảo local đã cài đặt mountebank 
    - Chạy lệnh `docker restart mountebank`
    - Kiểm tra kết quả tại: http://localhost:8585/imposters

#### 3. Thao tác với Stub
Stub là thành phần trong mỗi imposter. Mỗi stub trong imposter có vai trò tương 
đương với một API của mock server.

3.1 Để thêm stub ta thực hiện các bước sau:
- Tạo file json để cấu hình mock API. Ví dụ: Tạo stub `delete_object.json` có nội
dung như sau:

```json
{
  "responses": [
    {
      "is": {
        "statusCode": 204,
        "headers": {
          "Content-Type": "application/json"
        }
      }
    }
  ],
  "predicates": [
    {
      "equals": {
        "path": "/api/dcim/sites/2",
        "method": "DELETE"
      }
    }
  ]
}
```

- Import stub vào imposter tương ứng:

```json
{
  "protocol": "http",
  "port": 8383,
  "name": "imposter-1",
  "requests": [],
  "stubs":
    [
      <% include ./dcim/sites/delete_sites.json %>
    ]
}
```
- Test trên localhost:
    - Đảm bảo local đã cài đặt mountebank 
    - Chạy lệnh `docker restart mountebank`
    - Kiểm tra nội dung API: http://localhost:8585/imposters
    
Vậy là ta đã giả lập một API với url có request và response được cấu hình như 
trên.

3.2 Sửa/xóa stub 
Các thao tác tương tự với việc sửa/xóa imposter 