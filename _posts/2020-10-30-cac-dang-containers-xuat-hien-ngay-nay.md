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
- Có những công cụ triển khai container nào đang phát triển? Docker có nằm trong số đó?
  - External tool --> CRI --> high level CR --> low level CR ----> Container

2. Container
  - Container là gì? Đặc điểm container
    - Namespace
    - Cgroup
  - 2 dạng containers: OS container và Application Container
  - Container nằm ở đâu trong kiến trúc máy tính
  - Nên áp dụng Công nghệ container khi nào

3. Container runtime
  - Low level CR
    - Low level CR là gì?
    - Cung cấp những tính năng gì?
    - Sử dụng khi nào?
    - Low level CR phổ biến: RunC, Kata Container
  - High level
    - High level CR là gì?
    - Cung cấp nhưng tính năng gì? Khác gì với Low level CR?
    - Sử dụng khi nào?
    - High level CR phổ biến: CRI-O, Containerd, Docker Engine,...

4. Container runtime interface (CRI)
  - CRI là gì? Khác gì với CR
  - Cung cấp những tính năng gì?
  - Sử dụng khi nào
  - CRI phổ biến: Kubernetes

5. Kết luận
- Container không phải là docker. 
- Có nhiều dạng công cụ hỗ trợ triển khai container. 
- Việc triển khai container có thể được thực hiện bởi các công cụ khác nhau nhưng
đều dựa trên một chuẩn container (OCI) thống nhất. 

---



Chào các bạn, là mình, tranglua đây! 

Như các bạn đã biết, Container hiện nay đã trở thành một khái niệm vô 
cùng quen thuộc không chỉ với cộng đồng OpenInfra chúng mình rồi phải
không. Khi nhắc tới container, mọi người thường liên tưởng ngay tới 
Docker. Đúng vậy, Docker gần gũi với chúng ta tới mức rất nhiều bạn đã
dần lầm tưởng Docker là container, container chính là Docker rồi. Vậy hai khái niệm này có phải là một không nhỉ? Hôm nay mình và các bạn sẽ 
cùng nhau làm rõ vấn đề này nhé!

Đầu tiên, Docker hoàn toàn không phải container. Chúng ta không thể gộp hai khái
niệm này làm một nhé. Cụ thể thì:
  - Docker là một nền tảng cho phép chúng ta triển khai các ứng dụng
  trên môi trường container. 
  - Container là một nhóm bao gồm các tiến trình chạy trên một single host
  có những đặc tính chung.

Chính vì vậy, hiện nay ngoài Docker ra, đã có rất nhiều các nền tảng công cụ hỗ trợ triển
khai công nghệ container như Containerd, RunC, LXD, KataContainers,... 

### Container và các dạng container
#### Container
Như mình đã trình bày phía trên, container là nhóm các tiến trình chạy trên một
single host có những đặc tính chung dựa trên các layer của kiến trúc hệ thống. 
Những đặc tính chung ấy có thể là: CPU, storage, OS kernel,... 



Có 2 loại container: `OS Container` và `Application Container`. `OS container` là giải 
pháp chạy đa tiến trình tập trung chủ yếu vào việc cung cấp một môi trường runtime 
(OS) chia sẻ OS kernel nhưng độc lập về vùng tài nguyên người dùng. Khá giống với virtual 
machines, chúng ta có thể cài đặt, cấu hình và chạy các ứng dụng, thư viện,...
Bất kỳ các đối tượng nào có mặt trong container đều chỉ có thể nhận biết được vùng
tài nguyên được gán cho container đó. OS container thường được sử dụng để 
triển khai các ứng dụng có dạng monolithic truyền thống khi triển khai. 
Khác với OS Container, `Application container` cho phép chạy đơn tiến trình với
mục đích chính là hỗ trợ các dịch vụ nhỏ (microservice), dễ dàng triển khai các 
ứng dụng phân tán. Lúc này, mỗi ứng dụng có thể được chia ra nhiều tasks đóng gói 
thành các images. Bằng cách sử dụng các images này, mỗi task sẽ được triển khai 
trên một container một cách độc lập. Bên cạnh Docker, chúng ta có thể kể đến một 
vài công cụ phổ biến khác giúp triển khai `Application container` như Kubernates, 
CRI-O,...

Như vậy, nếu như bạn muốn đóng gói và phân tán ứng dụng của bạn thành nhiều thành phần,
`Application container` sẽ giúp bạn làm điều đó cực kỳ tốt. Còn nếu bạn cần một hệ điều
hành cài đặt nhiều thư viện, ngôn ngữ, database,... khác nhau thì `OS container` là một
lựa chọn phù hợp hơn cả.

##### Tổng kết


---
Reference

Blog

0. https://github.com/saschagrunert/demystifying-containers
1. https://medium.com/docker-containers/types-of-container-technologies-cbeb6e09aaa0
2. https://jfrog.com/knowledge-base/6-alternatives-to-docker-all-in-one-solutions-and-standalone-container-tools/
3. https://www.cio.com/article/2924995/what-are-containers-and-why-do-you-need-them.html 
4. https://www.contino.io/insights/beyond-docker-other-types-of-containers 
5. https://learn.g2.com/container-technology 

Articles

6. https://www.researchgate.net/publication/325534952_Containers_for_Virtualization_An_Overview
7. https://www.researchgate.net/publication/310514065_A_Performance_Study_of_Containers_in_Cloud_Environment
8. https://www.researchgate.net/publication/316903410_Cloud_Container_Technologies_a_State-of-the-Art_Review
9. https://core.ac.uk/download/pdf/147608158.pdf

Books

10. https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/ebook/vmware-press-ebook-on-containers-and-kubernetes.pdf
11. https://www.mindtree.com/sites/default/files/2019-01/Containers%20in%20enterprise.pdf
12. https://www.qcmtech.com/wp-content/uploads/2017/09/HPE-pub-10010-Containers-for-Dummies.pdf
13. https://linuxacademy.com/templates/default/assets/pdf/containers-for-everyone-ebook.pdf 
14. https://www.linuxjournal.com/sites/default/files/2018-11/GeekGuide-Puppet-Containers101.pdf 
15. https://www.liquidtechnology.net/wp-content/uploads/2018/01/Container-Technology.pdf

Others

16. Containers at google: https://cloud.google.com/containers
17. Containers on Compute Engine: https://cloud.google.com/compute/docs/containers
18. https://linuxcontainers.org
19. https://www.docker.com/resources/what-container 
20. Containers on AWS: https://aws.amazon.com/vi/containers/

--------