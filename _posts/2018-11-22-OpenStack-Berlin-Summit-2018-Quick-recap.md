1. Tổng hợp chung

- Một số công ty telco đã "biến mất" trong khu vực Marketplace, chỉ có ZTE và Nokia có booth, E//, AT&T, Huawei, etc. không thấy đâu mặc dù vẫn có nhưng bài presentations.
- Foundation vẫn đang tiếp tục con đường trở thành một multi-project foundation trong đó OpenStack chỉ là một trong những project. Summit tiếp theo sẽ được đổi tên thành: Open Infrastructure Summit.
- Những dự án hiện tại của foundation chính là: OpenStack, Zuul, StarlingX, Airship, Kata container, trong đó thì OpenStack và Zuul được nhận định là đã "trưởng thành".
- Các topics kỹ thuật trong summit lần này được phân làm 3 khu vực chính:
	- Container: Có rất nhiều session về các tính năng cần thiết để chạy các containerized workload khác nhau trên cùng một môi trường OpenStack.
	- Fast forward upgrade of infra: Rất nhiều session về vấn đề này nhưng nhìn chung thì nhận định vẫn là: Khó của Nam Cường :d.
	- Edge Cloud: Các session đều chỉ ra được một tương lai khá là sáng rạng về use cases của edge clouds. Có rất nhiều thảo luận về các yêu cầu cụ thể của một MVP mà Edge 		Computing Group định nghĩa.
- Các kỹ sư của Redhat không vui lắm với việc bị IBM mua lại nhưng tất cả đều tin rằng, mọi hoạt động của họ không thay đổi :d.
- Deutsch Telecom đang viết một prototype về 5G Edge Cloud sử dụng MobileEdgeX: https://mobiledgex.com/


2. Các vấn đề cụ thể:

- Phiên bản Rocky: 
	- Released vào 8/2018
	- hightlights: https://releases.openstack.org/rocky/highlights.html

- Phiên bản Stein:
	- Released vào 4/2019
	- Mục tiêu là sử dụng python 3 as default và automated preupgrade checks.
- Airship:
	- Kế hoạch là build một release candidate và một release 1.0
	- Release candidate features:
		- Security out of the box
		- Resiliency
		- Scaled Operations
		- Predictable upgrades
		- Orchestration: Batteries Included
		- Repetable Multi-site Deployments
	- Release candidate features:
		- Ironic Integration 
		- Friendly YAML Generation 
		- Multi-OS support
		- Hardware/network auto-discovery

- Zuul:
	- Hỗ trợ nodepool driver cho việc quản trị các node trong Zuul cũng như hỗ trợ các Zuul job
	- GUI dựa trên React
	- Hỗ trợ các hệ thống review code như Gitlab

- Kata:
	- Phiên bản 1.1.0 released July 2, 2018 
	    	- PPC64 support. 
	    	- Shared PID namespace. 
	- Phiên bản 1.2.0 released August 8, 2018
		- VSOCK support, making the proxy an optional component.
		- VM Factory support for faster boot time 
	- Phiên bản 1.3.0 released September 13, 2018
		- Containers get entropy via virtio-rng. This creates a higher quality randomness for random number generation.
		- Kata Agent has optional seccomp support, which is the first step to enabling seccomp in Kata Containers in the future.  
	- Phiên bản 1.4.0 - Expected mid-November
		- TC-mirroring based networking, along with IPVLAN and MACVLAN support
		- NEMU hypervisor support
		- Extended tracing support
		- Optional seccomp in guest

- Edge:
	- MVP được tách thành 2 option chính về kiến trúc. Một hướng đi là hỗ trợ isolation của các instance của các edge cloud - chủ yếu được phát triển với StarlingX, một hướng khác là không cần đến sự hỗ trợ của network isolation - được phát triển bởi Oath. Tuy nhiên cả hai đều cần các sự thay đổi mới trong keystone và glance.
	- Airship Armada được sử dụng một vài dự án để quản trị Helm charts. Những phần khác thì dường như ko thấy có use cases, ngoại trừ AT&T.
	- StartlingX đang trong quá trình tìm kiếm các yêu cầu cho các ứng dụng container hóa.

- Containers:
	- Kata container đang có những bước phát triển rất tố, các bước tiến về dev nhanh kể từ summit trước tuy nhiên việc đưa vào sử dụng vẫn còn hạn chế.
	- Tightly coupled tới các máy ảo Linux tại thời điểm hiện tại và yêu cầu có các agent trong VM. Do đó với các non-Linux VM, kata cần thiết phải có các agent implementation.
	- K8s dường như là người chiến thắng trong câu chuyện về container orchestrator, có rất nhiều session nói về việc triển khai k8s trên OpenStack cũng như baremetal.
	- Rất nhiều các hãng lớn như Google, IBM, etc. offer các giải pháp của họ sử dụng k8s trên Ubuntu.
	- Việc triển khai k8s trên baremetal đang trở nên phổ biến với các usecase liên quan đến edge nodes.
	- Hypernetes là một dự án liên quan đến multi-tenancy trên k8s, kata container cũng thảo luận với hypernetes như là một phương án để tăng sec.

- Fenix:
	- Fenix có kế hoạch sử dụng Tacker như là một VNFM trong môi trường testing functional của OpenStack. Tuy nhiên vẫn chưa có một implemetation cụ thể nào trong Tacker.

- Blazar:
	- Vẫn được lead bởi Docomo.
	- Các features dự kiến để implement:
		- IP và VLAN resource reservation.
		- Cinder volume reservation.

	
