---
layout: post
title: OPNFV-Installer-Compare
date: 2017-05-03
type: post
published: true
status: publish
categories:
- Chia sẻ kinh nghiệm
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

Apex
Pros
This is actually using TripleO of Redhat to deploy VIM. Undercloud will be installed on a virtual machine running in Jump Host and then from that Jump Host (JH), tripleO will deploy overcloud.
Same OS (CentOS 7) on JH and nodes
RPMs are available from artifacts so no need to reinstall OS on JH. Actually, artifacts are the local copies of all the RPM packages.
Useful e.g. when upgrading Apex version
Same interface for PXE boot and Openstack admin/MGMT network. Therefore we do not need two NICs for them or VLAN segmentation configured in switch.
JH will forward the internet access through admin network, therefore there is no need of direct Internet access to the nodes.
Functional test is running inside docker that is running on JH. There is no need of iptables modifications for docker running.

Cons
No GUI for deployment
Configuration files (.yaml files) are not always clear, e.g. ’bridged’ parameter required for certain networks in network_settings.yaml
During install undercloud may lose its DHCP-lease which causes installation to hang. If it happens, you need to log into the undercloud VMand then restart the libvirt interface which is mostly is eth0.
Root login disabled for nodes. It is the tripleO configuration default with the user is heat-admin. In this case, we can access to nodes and switch to root user by using “sudo -i”
Does not support JH reboot (or power outage), undercloud does not get IP addresses
For a good user experience the opnfv-util function has to be used (from JH). IP addresses are hidden inside the undercloud, so without opnfv-util you have to first login to undercloud and then ”ssh heat-admin@<node_IP>”.

Fuel
Pros
User friendly GUI for deployment
No .yaml file configuration. This information should be checked again??
Sanity checks before deployment, e.g. checks that there is connectivity between every node on each NIC
Public IP range does not have to be continuous
All logs can be found in Fuel dashboard (every node and every openstack service log)
Log files are also stored in fuel node. They are collected through rsyslog 
Extremely useful when debugging
Fuel has its own health check which can be run from the dashboard
Supports JH reboot
ssh-keys automatically setup with host names configured, easy to access: ssh node-1, ssh node-2 etc
Cons
PXE and admin network are separate, meaning one additional NIC is required or VLAN segmentation has to be used. Not really a problem but requires switch config.
”Local” installation not as easy as in Apex. Supposedly it should work by changing the repository mirror address in Fuel dashboard to the IP address of the JH, but sometimes it does not work. Could be a network configuration problem.
Functional test docker does not work out of the box. Iptables need some additional configurations of postrouting rule for docker subnet.
- ( iptables -t nat -A POSTROUTING -s IP_ADDRESS -j MASQUERADE )
- Might be easier to run functional test from a controller but Docker is not installed within controller.
JH runs on CentOS 7 while nodes run on Ubuntu
Slower DHCP than Apex: vping test takes roughly 140s in Fuel; 80s in Apex. This information should be checked again?
- Most likely an ODL problem
RPMs not available as artifacts, JH has to be reinstalled when upgrading
Compass4NFV
Pros
In CI it has the best results and fastest vping run time
Cons
Unclear installation instructions
Hard-coded parameters
Huawei uses 3 NICs (BMC, PXE, ”external”), where all openstack networks are on the same NIC (external) with VLAN segmentation. Problems occur when you have more NICs (as we have in our lab)
Poor support, e.g. no IRC-channel
Conclusion
Both Apex and Fuel are solid options and they perform functionally about equally well (in pre-Colorado)
Fuel is easier to install and the dashboard is useful even after deployment
During the last month or so Apex has improved a lot, so in the future it might be the better choice
It would be really interesting to see if Compass performs as well in our environment as in Huawei’s pod. It doesn’t seem to have the same ODL problem as Apex and Fuel have; vping only takes roughly 20 seconds to complete. The reason behind my fixation on vping is that several tempest testcases fail in Apex/Fuel because of ssh-timeout since it takes too long for VMs to get an IP.
Unless you manage to get heavy support from Huawei I would recommend to stay away from Compass until they support multi-NIC better

VietStack team
