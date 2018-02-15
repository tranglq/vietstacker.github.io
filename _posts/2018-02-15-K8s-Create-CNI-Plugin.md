---
layout: post
title: K8s-Create_CNI-Plugin
date: 2018-02-15
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

## Let's have a quick look about CNI at some points:

- K8s accepts CNI specification and its plugins to orchestrate networking whereas Docker uses libnetwork plugin interface.
- There are some main steps happening in CNI:
	  - Container runtime creates a network namespace. However, docker does not create a symlink to /var/run/netns therefore it may be refusing that there is no result when checking "ip netns"
	  - Provisioning network is provided by a JSON file. CNI will refer to the "type" field within JSON file which defines plugin and then invoke the corresponding plugin executable file.
	  - Plugin creates vethpair and assign IP address to the interface.

There is alot of information about CNI on Internet so we should not waste time for it here. Let's rock it on with a real test.

## Test Implementation

### Enviroment

- I have here myself a testbed which is k8s multinode. I created it with kubeadm for testing and used flannel or overlay network. All the nodes are using Ubuntu 16.04

### What is the test about?

- We will create a pod which has initially 2 interfaces: One is loopback, another one is eth0 which is actually created and attached to flannel network.

- After that, we will attach another interface to the running container of that pod.

- I create an executable that will help us to do that. This executable is not only for flannel network but also for others. It means that when you run it, you just need to provide the cni plugin you want, the rest will be done automatically.

### Implementation

- Here is the link of code: https://github.com/vietstacker/K8s-CNI-Plugin
- Use go dep to create dependencies for main.go and go build to create executable file, for instance: my executable file called "main".
- Here i am going to add a new interface of flannel, you can add whichever type of cni network you want, calico, weaver, etc.
- You have to download the executable file of cni plugin and put it to the $PATH_OF_CNI_PLUGIN directory.
- The command you should run is: 
	
   sudo CNI_COMMAND=ADD CNI_CONTAINERID=$CONTAINER_ID CNI_NETNS=/var/run/netns/$CONTAINER_ID CNI_IFNAME=eth12 CNI_PATH=$PATH_OF_CNI_PLUGIN ./main <vietstack.conf
	

So.............. Have fun !


Tutj/VietStack

