---
layout: post
title: Boston_UseCase_Sumup
date: 2017-06-09
type: post
published: true
status: publish
categories:
- Tech
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


### Tổng kết meetup Boston
 
Đây là một số tổng kết ngắn gọn về việc ứng dụng OpenStack cũng như xây dựng đường hướng, mục tiêu xoay quanh hệ sinh thái OpenStack của các công ty lớn. Bản tổng kết sâu về kỹ thuật sẽ được gửi tới các bạn trong meetup tới đây của cộng đồng VietOpenStack.
 
 
#### Nhận xét chung:
 
- IaaS Cloud là một trong những keynote trong summit lần này. Các nhà cung cấp dịch vụ sẽ deploy các IaaS cloud cho người dùng và chính bản thân người dùng sẽ quản trị và xử lý các vấn đề liên quan đến nó
 
- Edge computing use cases được nhắc đến khá nhiều trong sự kiện lần này tuy nhiên một cái nhìn rõ rệt nhất về kiến trúc của nó thì vẫn chưa được đưa ra. Rất nhiều ý kiến cho rằng, với Edge computing thì Linux container sẽ được định hướng cho sự phát triển của nó chứ không phải ảo hóa dựa trên hypervisor.
 
- Các vấn đề về container được đưa đến trong mối quan tâm mới, nhận được rất nhiều sự quan tâm chính là việc sử dụng contaier cho việc delivery, upgrade và khả năng tương tác lẫn nhau giữa các dịch vụ của OpenStack.
 
- NFV một lần nữa lại là cái tên được nhắc đến khá nhiều với OpenStack được sử dụng cho private cloud trong lĩnh vực telco. Tuy nhiên vẫn tồn tại một vấn đề liên quan đến sự tương tác giữa các operators, vendors trong telco với cộng đồng OpenStack do đó các operators trong telco nhận ra rằng, cách duy nhất để đạt được các yêu cầu phù hợp với telco một cách nhanh nhất, chính xác nhất chính là bản thân họ phải có các developers cho OpenStack.
 
 
#### Use cases


	##### 1. Verizon
 
- Một sản phẩm nổi trội của Verizon chính là VNS (virtual network services). Họ muốn hướng tới một mô hình gọi là: Masively Distributed OpenStack, nó kha khá tương đồng với Edge computing.
 
- Hiện tại trong mô hình triển khai VNF của Verizon:
	* Orchestrator quản lý toàn bộ VNFs
	* Resource orchestrator: Họ dùng giải pháp của EEricsson (cái này hồi trc thì là thế, hiện tại cần kiểm chứng)
	* Service orchestrator: Giải pháp riêng của Verizon, chính là cái VNS phía trên.
	* Multi-layer control: Họ đang hướng tới Tricircle
 
	##### 2. AT&T
 
- AT&T đóng góp khá nhiều cho cộng đồng OpenStack trong mảng container (OpenStack Helm, )
 
- Xác định kỷ nguyên tiếp theo là của Edge Computing. Đây cũng chính là mảnh đất màu mỡ cho OpenStack.
 
- Các use cases rõ rệt cho Edge Computing chính là IoT, AR/VR, cloud RAN, vCPE...
 
- AT&T đang có kế hoạch sẽ sử dụng kubernetes để chạy các dịch vụ OpenStack với sự trông đợi như dưới đây: 
	* Cung cấp một tool thuận tiện cho việc triển khai OpenStack.
	* Overhead sẽ giảm dưới góc độ quản lý
	* Modular scaling
	* Hỗ trợ resiliency và scale trên diện rộng
	* Hitless/in-place upgrade
 
- AT&T cũng đang có xu hướng sẽ sử dụng Magnum trong thờ gian tới
 
 
	##### 3. Redhat:
 
- Redhat đặc biệt quan tâm đến các giải pháp cloud liên quan đến Telco trong thời gian gần đây, một trong số đó là giải pháp về multi-sites để đáp ứng các yêu cầu trong telco. Redhat đưa ra một số use case như sau: 
 
	* OpenStack WANWide: Một single OpenStack deployment sẽ quản lý các remote compute resources được đặt tại Edge, các controllers sẽ đc deployed master/slave tại central location. Giải pháp này đơn giản, nhưng ko thể scale, đặc biệt có vấn đề về latency, bandwidth
 
	* Nova Cells: - Một top-level cell sẽ chạy nova-api. Các child cell sẽ chạy các dịch vụ của Nova, ngoại trừ nova-api. Trong các cell sẽ có nova-db, message queue. Đây cũng chính là use case của CERN.
 
	* OpenStack regions: - Mỗi một region sẽ là một deployment hoàn chỉnh của OpenStack, ngoại trừ Keystone được chạy ở central. Central keystone này sẽ authenticate các OpenSTack cloud. each cloud.
 
	* OpenStack Multi-Clouds: - Các cloud OpenStack riêng biệt nhau, phục vụ cho từng khu vực.
 
	* OpenStack Federation: Cho phép các users của các cloud OpenStack khac nhau  có thể sử dụng các nguồn lực của nhau. Sử dụng trusts giữa identity providers và service providers. Đây cũng chính là use case của Canonical.
 
	* OPNFV Multisite project: - Cung cấp  VNF high availability giữa các multi-region OpenStack clouds. Kingbird là một project liên quan đên vấn đề này
 
	* Tricircle: Cái này các bạn có thể check trên Internet nhé :)
 
 
	##### 4. T-Mobile:
 
- Đã bắt đầu sử dụng OpenStack cách đây khá lâu khi mà Ericsson chính là công ty cung cấp Ericsson Cloud Environment tuy nhiên hiện đang gặp vấn đề về việc upgrade các phiên bản OpenStack, hiện tại họ đang dậm chân tại các phiên bản cũ của OpenStack.
 
	##### 5. Ericsson:
 
- Có vấn đề về các features liên quan đến networking trong OpenStack. Ericsson đóng góp cho cộng đồng một feature khá hay chính là Vlan-trunking mà bản thân feature này vẫn đang gặp nhiều vấn đề lớn.
 
 
	##### 6. Centurylink:
 
- Đang tập trung giải quyết các use case liên quan đến DPDK, SR-IOV, các features vô cùng quan trọng trong việc tối ưu hóa hiệu năng của networking. 
 
 
	##### 7. Nokia:
 
- Tập trung phát triển bộ 3 CBIS, CBAM, CBND dựa trên nền tảng OpenStack.
- Sắp tới sẽ đầu tư phát triển thêm sản phẩm Container as a service.
 
 



6/2017


VietStack team
