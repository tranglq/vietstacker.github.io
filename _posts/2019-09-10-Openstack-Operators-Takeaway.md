Deployment tool:
- Con số chia đều cho 3 ông: OpenStack Ansible, Kolla Ansible và TripleO.
- Ý tưởng chạy OpenStack trên k8s nói chung khá là "hài hước" vì các operators họ nói chung chả hứng thú gì lắm. Vì đơn giản, cost để vận hành/maintain openstack đã khá tốn kém rồi, h lại thêm chi phí cho k8s nữa, nói chung là không được hay ho cho lắm. Cơ mà điều này lại đi ngược lại với mong muốn của Airship - một dự án để deploy OpenStack trên k8s của OpenStack foundation. Cho nên ta mới thấy rằng, người dùng chính là người quyết định xu hướng.

OS: Nhìn chung phân bổ khá đều cho CentOS/Rhel và Ubuntu.

Monitor và Cloud testing: 
- Thực sự thì đây vẫn là một miếng bánh thị trường khá béo bở cho những ai muốn khởi nghiệp trên SharkTank :).
- Có một số cuộc thảo luận về việc đặt các agent chạy trên hypervisor để monitor cũng như việc đặt các centralized monitoring tool ở đâu để cho dù cloud có die thì các monitoring tool vẫn hoạt động.
- Không nhiều cloud operators có một staging env cho testing vì vấn đề chi phí.

Accounting và Billing:
- Đây cũng là một thị trường béo bở cho các ae khởi nghiệp trên Sharktank. Các giải pháp mã nguồn mở về vấn đề này đều ko đáp ứng được yêu cầu.

Upgrades:
- Dễ dàng nhận thấy rằng, việc thiết lập một cloud mới, đánh giá và kiểm định, sau đó move các workload của production lên trên đó thì ít rủi ro hơn việc in-place upgrade. Việc in-place upgrade sẽ có nhiều rủi ro, e.g. phiên bản mới có vấn đề và ko roll-back được.
- Một vấn đề khá mệt mỏi khi upgrade cho OpenStack vì động chạm đến SDN vendor dependency. Ngoài ra chạy vẫn ngon, upgrade làm cái gì :)).

Networking:
- Lượng người sử dụng Octavia ngày càng tăng.
- Hầu hết vẫn sử dụng nhiều ML2/OVS. Số lượng sử dụng các công nghệ hỗ trợ cho hardware acceleration như OVS-DPDK hay SR-IOV thì chia đều cho đôi bên.
- Các công nghệ network truyền thống như Vxlan, L3 agent, floating ÍP vẫn dùng nhiều, DVR chưa được phổ biến
- Có khá nhiều operators sử dụng provider network hay shared VLANs.

Storage/Ceph:
- Nhìn chung thì hầu hết dùng Ceph upstream và external Ceph, ko dùng nhiều của các hãng.
- Các operators khá hạn chế "đụng","tune" đến ceph vì kinh nghiệm cho thấy họ đều phải "break" ceph cluster. Họ chỉ đụng đến trong trường hợp khẩn cấp.
