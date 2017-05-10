---
layout: post
title: OPNFV-Doctor-Functest
date: 2017-05-09
type: post
published: true
status: publish
categories:
- Chia sẻ kinh nghiệm
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


I has just spent a little time to run the doctor functest to see how functional it is. If you do not know what doctor is, please check the webpage of opnfv or see the link below:

https://blog.vietstack.vn/architecture-proposal-of-doctor-project/

As we know that in OPNFV Doctor, the abstraction of it is built upon the theories of couple components like Monitor, Inspector, Notifier, Controller. In the functest of Doctor, we can easily see how the doctor functionality works. In the functest, all the above components are simple implemented in different thread in order to expose the doctor function. Therefore, when we run functest and the the log/output of this test, we can see them working. 

The functest scenario:

- There is a monitor script to check the availability of compute host
- The test will “force-down” compute host.
- The Inspector will realize the availability of compute host and then send the notification to the consumer.
- The Consumer/Notifier will get the notification from the Inspector.

For more information about the full functional roles of those components, please check the Doctor project proposal on webpage of OPNFV of this link again:

https://blog.vietstack.vn/architecture-proposal-of-doctor-project/


The below logs are an example:

#### Monitor log:

```
+ echo '[./monitor.log]'
[./monitor.log]
+ sed -e 's/^/ | /' ./monitor.log
 | 2017-05-08 13:50:39,710 monitor.py 72 INFO   doctor monitor detected at 1494251439.71
 | 2017-05-08 13:50:40,107 monitor.py 74 INFO   ping timeout, quit monitoring...
+ echo
```
#### Inspector log:

```
+ echo '[./inspector.log]'
[./inspector.log]
+ sed -e 's/^/ | /' ./inspector.log
 | /usr/local/lib/python2.7/dist-packages/novaclient/client.py:278: UserWarning: The 'connection_pool' argument is deprecated in Ocata and its use may result in errors in future releases.
 |   warnings.warn(msg)
 |  * Running on http://127.0.0.1:12345/ (Press CTRL+C to quit)
 | 2017-05-08 13:50:39,719 inspector.py 100 INFO   event posted at 1494251439.72
 | 2017-05-08 13:50:39,719 inspector.py 101 INFO   inspector = <__main__.DoctorInspectorSample object at 0x7fd7463e9350>
 | 2017-05-08 13:50:39,719 inspector.py 102 INFO   **received data = [{"details": {"status": "down", "hostname": "overcloud-novacompute-0.opnfvlf.org", "monitor": "monitor_sample", "monitor_event_id": "monitor_sample_event1"}, "type": "compute.host.down", "id": "monitor_sample_id1", "time": "2017-05-08T13:50:39.710436"}]**
 | 2017-05-08 13:50:39,957 inspector.py 38 INFO   doctor mark vm(<Server: ofp_server>) error at 1494251439.96
 | 2017-05-08 13:50:40,014 inspector.py 38 INFO   doctor mark vm(<Server: doctor_vm1>) error at 1494251440.01
 | 2017-05-08 13:50:40,105 inspector.py 91 INFO   doctor mark host(overcloud-novacompute-0.opnfvlf.org) down at 1494251440.11
 | 127.0.0.1 - - [08/May/2017 13:50:40] "POST /events HTTP/1.1" 200 -
```

#### Consumer log

```
+ echo '[./consumer.log]'
[./consumer.log]
+ sed -e 's/^/ | /' ./consumer.log
 |  * Running on http://0.0.0.0:12346/ (Press CTRL+C to quit)
 | 2017-05-08 13:50:40,250 consumer.py 26 INFO   doctor consumer notified at 1494251440.25
 | 2017-05-08 13:50:40,251 consumer.py 27 INFO   received data = {"severity": "moderate", "alarm_name": "doctor_alarm1", "current": "alarm", "alarm_id": "0d9f0bf9-5181-49ac-8378-c8b4c75148b3", "reason": "Event <id=fbbe236c-64a8-4f19-a183-caf3bdc90424,event_type=compute.instance.update> hits the query <query=[{\"field\": \"traits.state\", \"op\": \"eq\", \"type\": \"string\", \"value\": \"error\"}, {\"field\": \"traits.instance_id\", \"op\": \"eq\", \"type\": \"string\", \"value\": \"758366d5-35f7-452b-90dd-4b34730105a6\"}]>.", "reason_data": {"type": "event", "event": {"event_type": "compute.instance.update", "traits": [["state", 1, "error"], ["user_id", 1, "2c26564230914e9ea134de794e079c44"], ["service", 1, "compute"], ["disk_gb", 2, 1], ["instance_type", 1, "m1.tiny"], ["tenant_id", 1, "d498d0054fd049eab4263796a9d45a77"], ["root_gb", 2, 1], ["ephemeral_gb", 2, 0], ["instance_type_id", 2, 10], ["vcpus", 2, 1], ["memory_mb", 2, 512], ["instance_id", 1, "758366d5-35f7-452b-90dd-4b34730105a6"], ["host", 1, "overcloud-controller-1.opnfvlf.org"], ["request_id", 1, "req-ee44b7ee-1e22-40a1-897b-017f194278e1"], ["project_id", 1, "d498d0054fd049eab4263796a9d45a77"], ["launched_at", 4, "2017-05-08T13:49:07"]], "message_signature": "a192c5c331e8ce4d8bd2da1ef3612a5cc47170a243b5dc5c14c33335ca127ec1", "raw": {}, "generated": "2017-05-08T13:50:39.991812", "message_id": "fbbe236c-64a8-4f19-a183-caf3bdc90424"}}, "previous": "insufficient data"}
 | 127.0.0.1 - - [08/May/2017 13:50:40] "POST /failure HTTP/1.1" 200 -
```

So, when we understand the abstraction of Doctor as well as the above example output, now the remaining tasks is designing/choosing the right candidates for each of components. If you have your own ideas, just implement it:). If not, check this link again as reference:

https://blog.vietstack.vn/architecture-proposal-of-doctor-project/



HAVE FUN

VietStack team
