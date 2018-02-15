---
layout: post
title: CD-Design-With-Helm-And-Tiller
date: 2017-08-23
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

## CI/CD Design With Helm And Tiller

### Exercise

As we all know that, when we design CI/CD for microservice using container technology, Helm chart is an useful tool for it. In this post, i will not talk about Helm and Tiller, suppose that we know what they are, if not, you can check the official web page of K8s or looking for it on Internet.

The problem i want to solve here is: Suppose that when we have multiple stages in CI/CD, for example, testing stage, deployment stage, pre-production stage (testing rolling-upgrade or performance test), etc. and then going to production environment which is actually a K8s cluster, we need to ensure that in the production enviroment (CD process) the security aspect should be ensure when Jenkins triggers the CD process. Why? Because Jenkins should not run inside the production environment, it should be run remotely at the standpoint of production environment. In this case, Jenkins should be able to talk to kubectl (the commandline of K8s cluster of production environment) remotely. But, it can only do that if it is provided with kubeconfig of cluster which is NOT secure if we distribute it outside of cluster. We by somehow need to ensure the protection of kubeconfig of K8s cluster.

### What is the role of Helm in this problem? 

In fact, helm - just k8s package manager - just talks to kubectl under the hood. If we want to use Helm for CD, we still have problem as described above.
So, the solution is this: We do not touch the default kubeconfig of K8s cluster but we create a temporary kubeconfig which is attached to a separate namespace of a temporary user. After that, we will attach tiller deployment running on K8s on that created namespace. Therefore when helm does something with chart, it will contact to tiller in that specific namespace - which is different to "kube-system" (the default namespace) namespace of tiller. Under the hood, kubectl will be run under the above created kubeconfig, not the default kubeconfig.


### For more details?

After discussing with Ami, we agreed on a solution and thank you for noting down, hope you can read my blog here :D. Here is his email: ami.mahloof@gmail.com.

The blog: <https://medium.com/@amimahloof/how-to-setup-helm-and-tiller-with-rbac-and-namespaces-34bf27f7d3c3>

Hope you enjoy it !!!


Tutj/VietStack

