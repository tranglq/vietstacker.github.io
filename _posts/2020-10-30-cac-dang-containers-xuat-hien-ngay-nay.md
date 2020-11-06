---
 layout: post
 title: "Container và các dạng containers đã có hiện nay"
 date: 2020-10-30 22:49:33
 image: '/assets/img/'
 comments: true
 tags:
 - container
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

Tóm tắt:

1. Đặt vấn đề
- Container có phải Docker không?
- Có những loại container nào?
- Có những công nghệ container nào đang phát triển ngoài Docker?
- Một số công ty công nghệ đang phát triển công nghệ containers
  - Acquia, Amazon web services, Google, DigitalOcean 
- Các loại container không chỉ Docker container

2. Container và các dạng container
  2.1. Container
    - Khái niệm
    - Mục đích sử dụng container:
      - Deploy ứng dụng nhanh chóng trên nhiều môi trường khác nhau nhưng nhất quán về code và cấu hình
      - Tăng cường tính linh động
      - Khả năng tự phục hồi, tự mở rộng
      - Giảm chi phí bằng cách tối ưu hoá tài nguyên
      - Cải thiện thời gian hoạt động
    - Container với mỗi đối tượng người dùng:
      - Deverlopers
      - IT management
      - Doanh nghiệp 
    - Một số nhà cung cấp công nghệ container:
      - Docker, LXC, rkt, FreeBSD Jails, Solaris Zones, LXD
      - Tỷ lệ được ưa chuộng, nguyên nhân
  2.2. Giới thiệu về 2 dạng containers cơ bản: 
  - OS container
    - Khái niệm
    - Mô hình kiến trúc của OS container: hình ảnh, mô tả
    - Vị trí, vai trò của OS container trong kiến trúc ứng dụng
    - Một số công nghệ OS container phổ biến: 
      - LXC/LXD
      - OpenVZ
      - Linux VServer,...
  - Application container
    - Khái niệm
    - Mô hình kiến trúc của application container: hình ảnh, mô tả
    - Vị trí, vai trò của OS container trong kiến trúc ứng dụng
    - Một số công nghệ application container phổ biến: 
      - Docker
      - CRI-O
      - Kubernetes...
  2.3. So sánh VM - OS container - Application container
  - Vị trí, vai trò trong kiến trúc ứng dụng
Kết luận

4. Kết luận
- Container không phải là docker. Docker là một nhà cung cấp công nghệ container phổ biến hiện nay. 
- Có nhiều dạng container, nhiều nhà cung cấp công nghệ container
- Tuỳ vào mục đích sử dụng có thể lựa chọn loại container phù hợp
và kết hợp sử dụng nhiều dạng container trong một mô hình kiến trúc
---
Reference

Blog
1. https://medium.com/docker-containers/types-of-container-technologies-cbeb6e09aaa0
2. https://jfrog.com/knowledge-base/6-alternatives-to-docker-all-in-one-solutions-and-standalone-container-tools/
3. https://www.cio.com/article/2924995/what-are-containers-and-why-do-you-need-them.html 
4. https://www.contino.io/insights/beyond-docker-other-types-of-containers 
5. https://learn.g2.com/container-technology 

Articles
1. https://www.researchgate.net/publication/325534952_Containers_for_Virtualization_An_Overview
2. https://www.researchgate.net/publication/310514065_A_Performance_Study_of_Containers_in_Cloud_Environment
3. https://www.researchgate.net/publication/316903410_Cloud_Container_Technologies_a_State-of-the-Art_Review
4. https://core.ac.uk/download/pdf/147608158.pdf

Books
1. https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/ebook/vmware-press-ebook-on-containers-and-kubernetes.pdf
2. https://www.mindtree.com/sites/default/files/2019-01/Containers%20in%20enterprise.pdf
3. https://www.qcmtech.com/wp-content/uploads/2017/09/HPE-pub-10010-Containers-for-Dummies.pdf
4. https://linuxacademy.com/templates/default/assets/pdf/containers-for-everyone-ebook.pdf 
5. https://www.linuxjournal.com/sites/default/files/2018-11/GeekGuide-Puppet-Containers101.pdf 
6. https://www.liquidtechnology.net/wp-content/uploads/2018/01/Container-Technology.pdf

Companies
1. Containers at google: https://cloud.google.com/containers
2. Containers on Compute Engine: https://cloud.google.com/compute/docs/containers
3. https://linuxcontainers.org
4. https://www.docker.com/resources/what-container 
5. Containers on AWS: https://aws.amazon.com/vi/containers/