---
layout: post
title: RoundRobin-Implementation-In-Microservices
date: 2017-05-30
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


## REACTIVE PROGRAMMING MODEL 



- We use an example of OFDM and TDMA from telco area to simulate/sample the idea of reactive programming model integrating with microservices model in software architect.

![alt text](https://github.com/vietstacker/vietstacker.github.io/blob/master/pictures/Fig-7-OFDMTDMA-system-model-with-round-robin-scheduling-Fig-8-Probability-of-packet.png)


- We need to follow the specifications of reactive programming model as well as reactive system here:
http://www.reactivemanifesto.org/

The reason that I decided to go with the model of OFDM/TDMA for this topic sampling because it has some below features that I myself think it somehow fits the specifications of reactive system in a simple implementation:

- We can take all the queues on the left to be queues of different inputs. Each queue will contain one type of input which is automatically update its state. For instance, queue1 can be the queue of virtual machines (vm1, vm2) running compute1. Each every time the vm1/vm2 changes its state (e.g. from RUNNING → STOP), an event about the change will be put into the queue.
- From each queue, we will get an event and then put it into another Round Robin logic which is (in the picture) the white small circle next to the queue. For instance, the white small circle is represented for a compute host, each circle will contain multiple events for multiple vm (e.g. some events like this: VM1-DOWN, VM2-MAINTENANCE, etc.). And these events are arranged in the Round Robin Logic.

The logic will be like this:

- White small circle: Compute object that takes event from queue
- Queues: Contains the event related to VM (e.g. VM1:DOWN, VM2:MAINTENANCE)

They will be implemented like this:

> Compute1 = {
> “VM1”: “DOWN”
> “VM2”: “MAINTENANCE”
> }


The big Round Robin logic is for compute host objects. Later on, the main logic will get each compute host objects from the big Round Robin and based on each of compute host object, it will get an event to execute.

So, to summarize, we will implement 2 Round Robin logic here, one per VM, one per compute host. The Round Robin logic per VM will be included inside the Round Robin logic of compute host.

And Why do we have the reactive system model here?? Because this implementation fits the general requirements of reactive system as shown with the main content as below:

- Elasticity: Since we have one queue that represents all the Vms on one compute host, we can easily increase the numbers of queue if the numbers of compute host increase.
- Responsiveness: It exposes through the event inside queue. If any vm changes state, an event in created and put into queue. This event will be solved when the main logic executes the relevant actions related to it.
- Resilience: In Python, queue is thread-safe therefore it is safe to use multi-thread in this implementation. Otherwise, we are going to implement the idea of microservices therefor it ensures the isolation of each service. What we should do is how to separate the whole logic into microservices.

- Messaging: Of course if we go with microservices architecture, we need communication channel for connection between micro services. It can be RestAPI, AMQP, etc.



#### ENVIRONMENT: 

Suppose that, we have a mapping from the queues like below as an example:

mapping = 
{ "Compute1" :{
      “VM1”: “DOWN”,
      “VM2”: “MAINTENANCE
      },
   
   "Compute2":  {...},
   
   "Compute3":  {....}
      .
      .
      .
}

What we have to do is putting all the vms into a round Robin logic as represented as white small circle and then putting all the computes into another Round Robin logic as represented as white big circle in the picture. And then later on, when the main logic wants to get the state of vm to do more actions, it will  go with Round Robin-like behavior as below:

Compute1-VM1 → Compute2-VM1 → Compute3-VM1→ Compute1-VM2 → Compute2-VM2 …..


#### IMPLEMENTATION:

In this example, I will show you two types of implementation. The first version is putting the whole logic in one service. The second version is separating the whole logic into microservices and setting the communication channel between them. 

**Version1**: 

I will put the whole logic of creating the result of queue output as a mapping and the logic to get the value of state per vm (as shown above) within one service. In this case, each policy of vm will have one queue which is used for storing the vm state based on policy.

- Main logic:


```python

import Queue


class VmStateQueue():
    def __init__(self):
        self.state_queue = Queue.Queue()

    def add_state(self, state):
        self.state_queue.put(state)

    def get_next(self):
        try:

            state = self.state_queue.get()
            # print "The state: ", state
        except Queue.empty:
            return None
        return state


class RoundRobin():
    def __init__(self, map=None):
        self.key_list = []
        self.current_index = 0
        if map:
            self.map = map
            self.key_list = sorted(list(self.map.keys()))
        else:
            self.map = {}

    def add(self, key, value):
        self.map[key] = value
        if key not in self.key_list:
            self.key_list.append(key)

    def delete(self, key):
        del self.map[key]


    def get_next(self):
        for _ in range(len(self.key_list)):
            value = self.map[self.key_list[self.current_index]].get_next()
            self.get_next_index()
            if value:
                return value
            return None

    def get_next_index(self):
        next_index = (self.current_index + 1) % len(self.key_list)
        self.current_index = next_index


```

- Test:


```python

import RoundRobin
import VmStateQueue
from unittest import TestCase


class Vm():
    def __init__(self, available):
        self.available = available

Vm1 = Vm(True)
Vm2 = Vm(True)
Vm3 = Vm(False)

q1 = VmStateQueue()
q1.add_state('healing')

q2 = VmStateQueue()
q2.add_state('scaling out')

q3 = VmStateQueue()
q3.add_state('scaling in')

q4 = VmStateQueue()
q4.add_state('dummy1')

q5 = VmStateQueue()
q5.add_state('dummy2')

q6 = VmStateQueue()
q6.add_state('dummy3')

q7 = VmStateQueue()
q7.add_state('UPDATE')

q8 = VmStateQueue()
q8.add_state('UPGRADE')

q9 = VmStateQueue()
q9.add_state('MIGRATE')

sub_map1 = {
        'pol1': q1,
        'pol2': q2,
        'pol3': q3
}

sub_map2 = {
        'pol1': q4,
        'pol2': q5,
        'pol3': q6

}

sub_map3 = {
        'pol1': q7,
        'pol2': q8,
        'pol3': q9

}

pol_rr_1 = RoundRobin(sub_map1)
pol_rr_2 = RoundRobin(sub_map2)
pol_rr_3 = RoundRobin(sub_map3)

map = {
    Vm1: pol_rr_1,
    Vm2: pol_rr_2,
    Vm3: pol_rr_3
}


class TestRoundRobin(TestCase):
    def setUp(self):
        self.rr = RoundRobin(map)

    def test_add(self):
        self.rr.add("foo", "bar")
        self.assertEqual(self.rr.key_list, [Vm1, Vm2, Vm3, 'foo'])

    def test_get_next(self):
        self.assertEqual(self.rr.get_next(), 'healing')
        self.assertEqual(self.rr.get_next(), 'dummy1')
        self.assertEqual(self.rr.get_next(), 'UPDATE')
        self.assertEqual(self.rr.get_next(), 'scaling out')
        self.assertEqual(self.rr.get_next(), 'dummy2')
        self.assertEqual(self.rr.get_next(), 'UPGRADE')
        self.assertEqual(self.rr.get_next(), 'scaling in')
        self.assertEqual(self.rr.get_next(), 'dummy3')
        self.assertEqual(self.rr.get_next(), 'MIGRATE')


```
**Version2**:

I will separate into two service, one to put events into queue and create a mapping, another is getting that mapping through communication channel like RestAPI, AMQP, etc. , put its items into roundrobin logics and then pulling out events to do next actions . I do not care about how  gets the mapping, what I care for is my service which is now putting the items of that mapping into roundrobin logic to ensure that the main logic will get events with a roundrobin-like behavior.

- Main logic

```python
from itertools import cycle, islice
import time
import Queue


def roundrobin(*iterables):
    pending = len(iterables)
    nexts = cycle(iter(iterable).next for iterable in iterables)
    while pending:
        try:
            for next in nexts: yield next()
        except StopIteration:
            pending -= 1
            nexts = cycle(islice(nexts, pending))

def check_vm_available(vm):
    if vm.available == False:
        return False
    return True

def put_vm_to_vm_queue(vm_rr_list):
    # Check if vm avalability
    for vm in roundrobin(vm_rr_list):
        if not check_vm_available(vm):
            vm_rr_list.remove(vm)

def put_jobs_of_vm_into_job_list(map, vm):
    policies = map[vm].keys()
    job_list = []
    for policy in roundrobin(policies):
        job_list.append(map[vm][policy])
    return job_list


def put_jobs_into_job_queue(job_queue, job_list):
    for job in roundrobin(*job_list):
        job_queue.put(job)


class RoundRobinService():
    def __init__(self, running_exec):
        # running_exec is a list of integer.

        self.job_queue = Queue.Queue()
        self.running_exec = running_exec

    def get_runing_execution(self):
        # return a list of all the current running executions

        while True:
            try:
                # a simulation to get the current running executions
                running_exec = self.running_exec.pop()
            except Exception as e:
                # sleep 5s is just an example
                time.sleep(5)
            else:
                return running_exec
                break

    @staticmethod
    def check_avai_slot_for_execution(running_execs, limit):
        # Limit is the maximum number of running execution
        # It can be configurable as "max_number_of parallel_executions" in config file

        if running_execs < limit:
            return limit - running_execs

    @staticmethod
    def get_vm(map):
        vm_rr_list = map.keys()
        return vm_rr_list

    def execute_job_per_vm(self, map, limit):
        # Get vm list
        vm_rr_list = self.get_vm(map)

        #
        put_vm_to_vm_queue(vm_rr_list)

        # Put jobs of each vm into a temporary job list
        temp_list = []
        for vm in vm_rr_list:
            temp_list.append(put_jobs_of_vm_into_job_list(map, vm))

        # Put all the jobs into job queue
        put_jobs_into_job_queue(self.job_queue, temp_list)

        # Get total running executions
        running_execs = self.get_runing_execution()
        free_slot = self.check_avai_slot_for_execution(running_execs, limit)

        if free_slot:
            for _ in range(free_slot):
                # Pick job to run from queue after roundrobin
                try:
                    job = self.job_queue.get()
                # Now run the job
                    print "Executing: ", job
                except Queue.empty:
                    break
                except Exception as e:
                    # LOG.debug("Failed to run scheduled job: ", e)
                    break


    def run(self, map, limit):
        count = 0
        while count < 3:
            self.execute_job_per_vm(map, limit)
            count = count + 1
            time.sleep(2)
```

- Test

```python

from unittest import TestCase
import RoundRobinService
import put_vm_to_vm_queue, roundrobin


class Vm():
    def __init__(self, available):
        self.available = available

Vm1 = Vm(True)
Vm2 = Vm(True)
Vm3 = Vm(False)

map = {
    Vm1: {
        'policy1': 'healing',
        'policy2': 'scaling out',
        'policy3': 'scaling in '
    },

    Vm2: {
        'policy1': 'migrate',
        'policy2': 'healing',
        'policy3': 'live-migrate'

    },

    Vm3: {
        'policy1': 'resize',
        'policy2': 'pause',
        'policy3': 'stop'
    }

}

class TestRoundRobin(TestCase):
    def setUp(self):
        self.running_exec = [1, 2, 3, 4, 5]
        self.f_service = RoundRobinService(self.running_exec)

    def test_get_runing_execution(self):
        self.f_service.get_runing_execution()
        self.assertEqual(self.running_exec, [1,2,3,4])

    def test_round_robin(self):
        vm_rr_list = [Vm1, Vm2, Vm3]
        result_list = []
        for item in roundrobin(vm_rr_list):
            result_list.append(item)
        self.assertEqual(len(result_list), 3)

    def test_queue(self):
        vm_rr_list = [Vm1, Vm2, Vm3]

        # Calling
        put_vm_to_vm_queue(vm_rr_list)

        # Then
        self.assertEqual(len(vm_rr_list), 2)

    def test_run(self):
        self.f_service.run(map, limit=10)
        self.assertEqual(0, self.f_service.job_queue.qsize())
```

#### NOTE:

- One thing I myself like the second version is that it decouples the logic and it follows the specifications of microservices. If the software is much more complicated, separating/decoupling logic based on business logic will isolate the dependencies between microservices as well as the organization of each team in the whole project. They do not depend or overlap the work of other teams, just implement their own works and getting the inputs from other services just through communication channel.


- This is only example for showing the way of implementing/using roundrobin logic and method of separating into microservices. There are totally a lots of problems behind this example and in fact, the software is not simple like this one therefore do not take it seriously into production.



30/5/2017

VietStack team
