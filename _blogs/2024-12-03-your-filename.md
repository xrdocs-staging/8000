---
published: false
date: '2024-12-03 14:22 +0530'
title: Cisco 8000 QOS Archetecture
author: Ram Mohan A M
excerpt: Cisco 8000 QOS archetecture
tags:
  - iosxr
  - cisco
  - Cisco 8000
  - SiOne
  - Cisco 8000 QOS
  - cisco 8000 policer
  - Cisco 8000 qos on bundle ports
---
## Cisco 8000 QOS Archetecture


## Introduction

Cisco 8000 series routers combines industry leading Cisco Silicon One, IOS XR networking operating system (OS), clean sheet chassis design and Crosswork automation tools to deliver high performance routers empowered with programmable transport infrastructure and network provisioning & monitoring tools.

Cisco 8000 is powered by Silicon One Network Processing Unit(NPU) which is evolving since 2019 , started with 10.8T capable 1st generation and 51.2T capable 3rd generation already rolled out in deployment.

Cisco 8000 router series has 3 form factors, Fixed, Centralised and Distributed Line card systems. there are many variants with different throughput capacity and port configurations already shipping in the field. There are different Cisco 8000 router variants addressing different roles in the network primarily Service Provider domain and Webscaler / Data Center domains. 

System buffering capability is one of the important architecture piece in NPU when it get designed to cater webscaler/DC and Service Provider use cases. Primarily 2 types of buffering needs during congestion situations in networks:  shallow buffering to absorb inflight packets over short range fiber links and deep buffering to absorb inflight packets over long distance fiber links. 

This document is targeted to help those readers who is interested in understanding below topics,
- SiOne NPU architecture
- Cisco 8000 QOS scheduling and buffering
- Cisco 8000 shaping & buffering 
- behaviour on Bundle ports
- Cisco 8000 policing behaviour and its aggregate behaviour on Bundle ports

## Silicon One (SiOne) NPU Architecture


![npu-arch.png]({{site.baseurl}}/images/npu-arch.png){: .align-center}

Silicon One (SiOne) ASIC is built with multiple slices where each slice comprise a set of Network Processing Engines(NPEs) for different packet processing functionalities like: Interface Groups (IFGs) - MAC provisioning, RxPP – receive packet processor, TxPP- Transmit packet processor and Traffic Manager – VOQs & Schedulers. And all slices function parallelly and perform the tasks associated with different packet processing tasks assigned by the run-to-completion processing blocks.

Every stage of packet processing has its associated data base lookups which is built by the system based on the configurations applied as well as the dynamically learned through different protocols. And these databases  supports the processing engine functions at each stages of packet processing by providing results for the corresponding lookups performed, feature processing etc.. And these data bases are hosted on different type of memory modules present on the system like TCAM, SRAM, HBM. These memory accesses  are well organised and optimised in such way SiOne ASIC achieves multibillion packet processing per clock cycle without compromising on high bandwidth performance.

SiOne has started its journey in 2019 and has many variants with different capabilities designed to cater to different segments in the networking paradigm. SiOne started by addressing webscaler spine roles then expanded to address service provider core, enterprise switching segment, DC interconnect and now stretching its legs towards SP aggregation, edge and access segments. Not stopping there and journey continues.  SiOne’s flexible architecture model helped in building new variants in short time. And it is evolving continuously. Number of slices and resource scaling can differ between different SiOne variants keeping fundamental architecture same. What varies between different variants is the bandwidth, serdes speed, size of different memory modules which directly influence features and associated service scale capability. 

Example, switching devices need high switching bandwidth , port density , shallow buffering with small routing table and feature scale. But on the other hand routing devices will demand medium to high bandwidth , deep buffering, feature richness and high scale routing table & services. SiOne flexible architecture design helps to build different variants in shorter time to cater to these use cases at minimal production cost.


### Life of packet through SiOne

![LoP.png]({{site.baseurl}}/images/LoP.png){: .align-center}


Let’s look into the packet forwarding stages in **SiOne**:

1. Packet arrives on network port hosted on the ingress IFG /Slice 
2. RxPP performs the packet lookup, ingress feature applications, and resolves the destination port 
3. RxPP enqueue the packet on the corresponding VoQ (destination port,traffic-class). Ingress scheduler kicks in, Ingress credit scheduler requests credits , 
4. When credits are available for the requested VoQ, packets are dequeued and switched across the SMS to the destination egress slice
5. On egress slice, TxPP performs egress feature processing and transmit the packets out of egress port with the right encapsulation added. Where the encapsulation data is carried in the NPU header which is built by the RxPP after the forwarding/resolution processing.
                                                                                                                                
**NOTE:-** Packet flow and associated processing remain same for intra-slice flows and inter-slice flows. all packets are switched through the same SMS 

On **distributed systems** ,  traffic incoming & outgoing slices can be on different NPUs across same Line card or different Line card  where Fabric mediates between ingress NPU and egress NPU for transferring packets from ingress to egress.  Since the packet transfer from ingress towards egress is strictly based on the credit grant received from the egress scheduler there no chance of fabric congestion or congestion at egress NPU side.


### VOQ architecture

VOQ (Virtual Output Queue) is a virtual representation of the output queues of a port at ingress side of the packet processing. While the physical buffer associated with the VOQs is on the input side of packet processing, it represents packets queued on the output side. VOQ based systems will have its VOQ representation programmed at ingress side of the packet processing. This programming happens after system boots up and creates the ports. Cisco 8000 systems programs VOQ replication for all default ports on the system. Same time system creates VOQs for the sub-interfaces only after a queueing policy applied on the sub-interface. And programmed VOQs will be deleted after sub-interface is deleted or the queueing policy is removed from the sub-interface

Every port has 8 hardwired Output Queues (OQ) on Cisco 8000. VOQs facilitate receiving traffic from any of the ingress ports hosted on any of the slices on the SiOne and transfer packets to the corresponding egress OQs of the respective egress port which is resolved by the forwarding lookup.


![voq-arch.png]({{site.baseurl}}/images/voq-arch.png)
{: .align-center}


Above figure depicts the VOQ replication aspects, set of 8 VOQs is programmed on every slices of the SiOne NPU for a given port . And same way, this replication happens on all slices on all SiOne NPUs across all line card modules in a distributed system. These VOQs uniquely identifies output queues of a port . This empowers the system to resolve most of the system information needed to forward out the packets at ingress processing stage. Thus optimises over all processing latency, read/write operations etc. which make the system more efficient in packet processing and power consumption. 

Traffic can come in on any port hosted on any of the slices which egress out of a port seen in the figure above, traffic get delivered to the OQs of the egress port from different slices which is facilitated by the VOQs representing that egress port &  scheduler present in each of the slices.

VOQ architecture advantages,

**Eliminate Head of line blocking** : on non-VOQ based architecture systems congestion on an egress port can effect different egress port that is not congested and this is because of the shared input queue model between multiple egress ports 

**Non-blocked fabric and efficient fabric bandwidth utilization:** packets move from ingress VOQs towards egress OQs only after ingress packet scheduler receives enough credit grants from the egress scheduler. So unlike traditional non-VOQ based systems there are no chances of further packets drops on the fabric or at the egress packet processing entities which ensures efficient fabric bandwidth usage and resources

**Single stage buffering :** traditional queueing architecture buffers packets in long-term storage memory modules at ingress processing stage as well at egress processing stage during the life of the packets through the ASIC. This is very inefficient way of ASIC resource utilisation and also results in more power consumption. But VOQ architecture buffers packets in long-term memory at ingress packet processing stage only. And minimising the number of dips into different memory/database resources  & associated read/write operations keeps over all electrical/electronic signalling at its minimum and hence low power consumption 

**low latency forwarding :**  Unlike traditional multi-stage forwarding model, VOQ architecture based system deduce most of the metadata required to push out  the packets at ingress processing stage itself leaving minimal processing requirements at the egress processing unit. With this,  system minimise the number of  read/write actions to different memory modules and associated data bases needed for overall packet processing through the system. All these ultimately helps the system to achieve the forwarding throughput with optimal latency where single stage buffering is also a major factor to achieve this.

## Cisco 8000 queueing & scheduling

Traffic can come in on any type of interfaces like main-interface, sub-interface, Bundle-main interface or Bundle sub-interface etc. at ingress side. But that does not matter from VOQ architecture and packet scheduling point of view. What matters is that out of which interface the packets have to egress out of the box.

How to enqueue incoming packets into the right VOQs and enforce differentiated QOS service on the system? We need combination of ingress and egress QOS policy application,

*Ingress policy application*

- Traffic need to be classified at ingress interface based on different fields in the packet header,
	- 	IP packet can be classified based on DSCP/Precedence values in IP header
	- 	MPLS packet can be classified based on EXP bits in the label 
	- 	Tagged L2 packets can be classified based on CoS/DEI bits in the VLAN tag

- Classified traffic need to marked to a ‘traffic-class’ (values range from 0 – 7) which maps to respective VOQ (VOQ0 – VOQ7). So ‘traffic-class 0’ maps to default VOQ ie, VOQ0 , ‘traffic-class 1’ maps to VOQ1 and so on.

*Egress policy application*

- Cisco 8000 supports 2 policy applications at egress side , remarking policy (DSCP/Prec, EXP remarking) and queueing policy
- Queueing policy application on egress interface decides the type of scheduling (strict priority, shaping, BWRR etc..) to be applied for each class of traffic and buffer management methods like, flat queueing threshold, curved queueing (RED) threshold, dual queueing thresholds.

### Default scheduling on main-interface

![default-sch-main.png]({{site.baseurl}}/images/default-sch-main.png)
{: .align-center}


Without any qos policy application at ingress and at egress,
- All classes of incoming traffic get mapped to default VOQ ie, VOQ0 (class-default) at ingress slice and hence mapped to OQ0 at egress side

### Default scheduling on sub-interface

System creates unique set of VOQs for sub-interfaces only if there is queueing policy attached to the sub-interface otherwise traffic destined out of sub-interface takes main-interface VOQs only.


![default-sch-subint.png]({{site.baseurl}}/images/default-sch-subint.png)
{: .align-center}

In this example, egress interface is a sub-interface for the traffic flow

- There is no qos policy application at ingress as well as at egress (sub-intf)
- All classes of incoming traffic get mapped to default VOQ ie, VOQ0 (class-default) of the main-interface

### Scheduling on main-interface with ingress traffic-class marking

Ingress classification and traffic-class marking policy is applied on ingress interfaces
###### Ex:-

![ing.png]({{site.baseurl}}/images/ing.png)
{: .align-center}




![sch-with-policy.png]({{site.baseurl}}/images/sch-with-policy.png)
{: .align-center}



- Incoming traffic get classified and mapped into respective VOQs
- If there is no egress queueing policy application on the egress interface then default scheduling is based on the profile: P1, P2 + 6 PNs
- This default scheduling profile can be overridden by user defined policy application at egress.

![eg.png]({{site.baseurl}}/images/eg.png)

- User-defined policy must have ‘traffic-class 7’ set with ‘priority level 1’
 - This is because the protocol control packets which are locally originated from the system are internally set with ‘traffic-class 7 and those packets has to be treated with highest priority. This is to make sure control packets are not compromised by data traffic in congestion situations
 - So its recommended not to use ‘traffic-class 7’ for usual data traffic just to avoid meddling of control traffic with data traffic in congestion scenarios
 

Lets look with another policy example as below, 
![full-pol.png]({{site.baseurl}}/images/full-pol.png)


- 4 classes of traffic coming at ingress side: PQ-1, PQ-2, INTERNET & Default-class
- Ingress policy classify each of the categories and mark the ‘traffic-class’ accordingly
- PQ-1 -> ‘traffic-class 6’, PQ-2 -> ‘traffic-class 5’, INTERNET -> ‘traffic-class 4’. And traffic which is not matching these 3 categories fall into class-default bucket
- As seen above, each classes get enqueued into corresponding VOQs: PQ-1 -> VOQ6, PQ2 -> VOQ5, INTERNET -> VOQ4 and rest to class-default (VOQ0). And rest of the VOQs are not being used in this case as seen above
- There is 2 level (HQOS) policy applied at egress side with a parent shaper of 10Gbps
- Child classes has 2 priority classes PQ-1 (TC6) & PQ-2 (TC5) and 2 normal priority classes (TC4 & default-class). There is TC7 also mentioned there which is a mandatory class to be mentioned in any user defined policies. TC7 carries all control plane traffic originated from the system


### What is default fairness in scheduling

Scope of VOQ replication on Cisco 8000 is at slice level as briefed in above sections. So  fairness is also at slice level.  What that means is, packet get scheduled out of VOQs across multiple ingress slices towards corresponding egress port OQs  in round robin fashion for a given traffic class. So bandwidth is scheduled equally for all slices for that given queue in congestion scenario keeping fair queueing at slice level.

As shown below figure, 

![Picture 1.png]({{site.baseurl}}/images/Picture 1.png)


egress port-7 is getting same class of traffic which is marked at ingress as ‘traffic-class 4’ from 3 ingress ports Port-1, 2 & 3. Ports 1 & 2 are in slice-0 and port-3 is on different slice. And all 3 ports are receiving traffic at 50% egress port bandwidth rate. So total ingress traffic which is resolved to go out of port-7 is 150% of port bandwidth and leads to port level congestion. Egress scheduler abide to slice level fair queueing and gives 50% worth credit of egress port bandwidth to both ingress slices. Thus, keeping fairness at slice level. So here, ports on slice-0 (port 1 & 2) shares the credit and consumes based on FCFS.

Another example as given in below figure ,

![Picture2.png]({{site.baseurl}}/images/Picture2.png)

all ingress ports are on different slices and egress scheduler grants 33.33% port bandwidth for each ingress slices.

Cisco 8000 supports fair queueing at port level on some fixed form factor systems based on Q200 ASIC (Second generation SiOne ASIC) to cater to some specific use cases of customers. VOQs have to be replicated at source port level to achieve port level fairness and hence more number of VOQ replications which in turn bring down over all VOQ scale.

## Traffic manager (TM)  & Packet Memory

Traffic Manager (TM) block in SiOne chip facilitate the overall packet management within the ASIC which includes, packet enqueuing into input buffer which is managed as VOQs which are programmed per destination/output port at slice level, dequeuing packets out of input buffer which is controlled by ingress & egress scheduling entities and streamlining over all congestion management & congestion avoidance with a set of well-defined rules.  

There are multiple memory modules associated with TM functions and major one is the packet memory. Fundamentally there are two types of packet memory in SiOne based Cisco 8000 routers,

Shared Memory System (SMS):
- SMS is on-chip memory
- Primary packet memory
- Fully shared memory

SMS memory is a shallow memory, usually in some 10s or 100s of MBs which varies between different SiOne ASIC variants. SMS is not meant to facilitate additional buffer room for packets while congestion and it is primarily to provide temporary stop while waiting for each processing engines to complete respective actions on the packets. So SMS provides shallow pit stop for packets while it is processed through different forwarding blocks in ingress processing stage.

High Bandwidth Memory (HBM): 
- Off-chip memory
- Secondary packet memory
- Fully shared memory

Cisco 8000 is designed with 8GB of HBM memory co-packaged with SiOne ASIC which is primarily used for deep buffering. So when we talk about queue-limit, RED etc. it is associated with HBM memory.

### Packet flow in non-congestion & congestion scenarios:
In non-congestion scenario packet get switched though SMS and HBM does not come into picture for such flows: packet gets enqueued into VOQs and dequeued out without any contention and associated latency. So packet movement out of VOQs towards egress OQs will happen at system designed switching speed.

![noncong.png]({{site.baseurl}}/images/noncong.png)


How about congestion scenario? Lets look into some of the criteria upon which system declares a VOQ residing in SMS memory as congested,

- Life of packet in packet buffer
 - Systems declares a VOQ  as congested when packets residing in it exceeds the time threshold set by the system
- Amount of buffers consumed
 - System declares a VOQ as congested when the number of buffers consumed by that VOQ goes beyond the maximum number of buffers set by the system
 
Above two are not the only conditions which need to be met before a VOQ mark for evictions. There are other supporting conditions like differentiation of priority vs normal classes, defining unique thresholds and rules for different memory threshold brackets etc..


Now lets see what happens while congestion,
- VOQ facilitating the packet flow get evicted to HBM
- And packet flow takes a path through HBM until the congestion persist

![conges.png]({{site.baseurl}}/images/conges.png)

- VOQs in HBM will get a default queue limit of 6ms if there is no user-defined queue limit in the respective class of the QOS policy applied on the egress interface
- VOQs in HBM can buffer additional packets up to the queue limit allocated to it and tail drop will happen if queue associated with the VOQ grow beyond the queue limit length applied to it. Tail drop can be caused by prolonged congestion or large bursts
- Packet flow experience latency during congestion because of the additional buffer pipe in its packet path
- VOQs move back to SMS after associated congestion is removed

### Where does buffering takes place??

Buffering is associated with the VOQs and its at ingress side of packet processing. 

### Deep buffering & Buffer management

SiOne has HBM memory module co-packaged with NPU module with an interposer providing low latency read/write access between NPU and HBM modules.

*All VOQs are served in SMS,*
![HBM-2.png]({{site.baseurl}}/images/HBM-2.png)

As seen above, 
-	There are many VOQs in SMS memory which is facilitating multiple packet flows through the system but none are congested
-	HBM is empty as all VOQs are non-congested and serving packet flows at its low latency switching speed in SMS



*some of VOQs get congested and evicted to HBM,*

![hbm-3.png]({{site.baseurl}}/images/hbm-3.png)

What is happening in above scenario,

-	Some of the VOQs in SMS are experiencing congestion
-	Congested VOQs get evicted to HBM and additional queue depth get applied to the VOQ while in HBM
o	Depth of queue depends on the user configured values in the respective class of traffic, default is 6msec if there is no explicit user configured queue limit
-	HBM start filling as more and more VOQs experience congestion simultaneously

*What happens after all buffers in HBM get filled up??*  Lets see,

![hbm-4.png]({{site.baseurl}}/images/hbm-4.png)

8GB HBM can get exhausted with just ~26 VOQs (based on raw bandwidth calculation) each configured with 300MB queue limit, 26x 300MB -> ~8GB. In deployments, there can be thousands of VOQs serving packet flows simultaneously and system can get choked easily by congesting just hundreds of VOQs simultaneously as per above calculation. 

How do Cisco 8000 service more VOQs in HBM simultaneously to keep optimal functioning of the system??

### Dynamic VOQ adaptation

![hbm-4.png]({{site.baseurl}}/images/hbm-4.png)

As seen above, all VOQs are getting the maximum configured queue limit. As more and more VOQs get congested , System adjusts the queue depth to lower brackets there by reducing the queue depth associated with each VOQs in the HBM. There by free up buffer space so that more and more VOQs can be evicted to HBM. 

As seen below, queue pipe associated with each VOQ adapts to lower size and welcoming more congested VOQs to HBM.

![hbm-5.png]({{site.baseurl}}/images/hbm-5.png)

System is designed with multiple brackets of buffer consumption regions. And each bracket is defined with maximum allocatable buffers per VOQ. Number of buffers per VOQ  get adjusted to next lower value as the number of buffers consumed within HBM crosses each bracket thresholds, this way system can scale higher number of VOQ eviction in worst congestion scenarios.

Ex:- 
-	26 VOQs evicted to HBM with each having a configured queue depth of 300MB
-	More and more VOQs started experiencing congestion 
-	System adjusts buffers allocated to 26 VOQs to lower value (as per the brackets defined ) which are already in HBM and free up HBM space to accommodate more VOQs
This way dynamic adaptation get initiated by TM based on HBM consumption crosses different brackets.


Queueing and Policing on Bundle Ether interfaces














































