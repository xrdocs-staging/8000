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
position: hidden
---
|Ram Mohan A.M., Technical Leader, Technical Marketing (raam@cisco.com)|  

{% include toc icon="table" title="Cisco 8000 QOS Architecture White Paper" %}


# Introduction

Cisco 8000 series routers combines industry leading Cisco **Silicon One**, **IOS XR** networking operating system (OS), clean sheet chassis design and Crosswork automation tools to deliver high performance routers empowered with programmable transport infrastructure and network provisioning & monitoring tools.

Cisco 8000 is powered by Silicon One Network Processing Unit(NPU) which is evolving since 2019 , started with **10.8T** capable 1st generation and **51.2T** capable 3rd generation already rolled out in deployment.

Cisco 8000 router series has 3 form factors, Fixed, Centralised and Distributed Line card systems. there are many variants with different throughput capacity and port configurations already shipping in the field. There are different Cisco 8000 router variants addressing different roles in the network primarily Service Provider domain and Webscaler / Data Center domains. 

This document is targeted to help those readers who is interested in understanding below topics,
- SiOne NPU architecture
- VOQ architecture
- Cisco 8000 QOS policy application model
- Cisco 8000 QOS scheduling and buffering
- Life of packet in VOQs: Enqueuing and Dequeuing
- Traffic manager and packet memories
- Cisco 8000 shaping & buffering 
- Dynamic VOQ adaption and congestion management
- Shaping & buffering on Bundle ports
- Cisco 8000 policing architecture and behaviour on Bundle ports

# Silicon One (SiOne) NPU Architecture


![npu-arch.png]({{site.baseurl}}/images/npu-arch.png){: .align-center}

Silicon One (SiOne) ASIC is built with multiple slices where each slice comprise a set of Network Processing Engines(NPEs) for different packet processing functionalities like: Interface Groups (IFGs) - MAC provisioning, RxPP – receive packet processor, TxPP- Transmit packet processor and Traffic Manager – VOQs & Schedulers. And all slices function parallelly and perform the tasks associated with different packet processing tasks assigned by the run-to-completion processing blocks.

Every stage of packet processing has its associated data base lookups which is built by the system based on the configurations applied as well as the dynamically learned through different protocols. And these databases  supports the processing engine functions at each stages of packet processing by providing results for the corresponding lookups performed, feature processing etc.. And these data bases are hosted on different type of memory modules present on the system like TCAM, SRAM, HBM. These memory accesses  are well organised and optimised in such way SiOne ASIC achieves multibillion packet processing per clock cycle without compromising on high bandwidth performance.

SiOne has started its journey in 2019 and has many variants with different capabilities designed to cater to different segments in the networking paradigm. SiOne started by addressing webscaler spine roles then expanded to address service provider core, enterprise switching segment, DC interconnect and now stretching its legs towards SP aggregation, edge and access segments. Not stopping there and journey continues.  SiOne’s flexible architecture model helped in building new variants in short time. And it is evolving continuously. Number of slices and resource scaling can differ between different SiOne variants keeping fundamental architecture same. What varies between different variants is the bandwidth, serdes speed, size of different memory modules which directly influence features and associated service scale capability. 

Example, switching devices need high switching bandwidth , port density , shallow buffering with small routing table and feature scale. But on the other hand routing devices will demand medium to high bandwidth , deep buffering, feature richness and high scale routing table & services. SiOne flexible architecture design helps to build different variants in shorter time to cater to these use cases at minimal production cost.


## Life of packet through SiOne

![LoP.png]({{site.baseurl}}/images/LoP.png){: .align-center}


Let’s look into the packet forwarding stages in **SiOne**:

1. Packet arrives on network port hosted on the ingress IFG /Slice 
2. RxPP performs the packet lookup, ingress feature applications, and resolves the destination port 
3. RxPP enqueue the packet on the corresponding VoQ (destination port,traffic-class). Ingress scheduler kicks in, Ingress credit scheduler requests credits , 
4. When credits are available for the requested VoQ, packets are dequeued and switched across the SMS to the destination egress slice
5. On egress slice, TxPP performs egress feature processing and transmit the packets out of egress port with the right encapsulation added. Where the encapsulation data is carried in the NPU header which is built by the RxPP after the forwarding/resolution processing.
                                                                                                                                
**NOTE:-** Packet flow and associated processing remain same for intra-slice flows and inter-slice flows. all packets are switched through the same SMS 

On **distributed systems** ,  traffic incoming & outgoing slices can be on different NPUs across same Line card or different Line card  where Fabric mediates between ingress NPU and egress NPU for transferring packets from ingress to egress.  Since the packet transfer from ingress towards egress is strictly based on the credit grant received from the egress scheduler there no chance of fabric congestion or congestion at egress NPU side.


## VOQ architecture

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

## Cisco 8000 QOS policy application model

Every system has QOS application model which defines the ingress & egress QOS policy applications and associated capabilities which end user can be explored with the system. 

Lets look at Cisco 8000 QOS application model , below is the depiction of the same:
![qos-model.png]({{site.baseurl}}/images/qos-model.png)

Cisco 8000 system has 3 policy configuration model ,
-	single policy at ingress side
-	2 policy model at egress: one for queueing and another for remarking. On Cisco 8000 remarking of DSCP/Precedence, MPLS-EXP can be performed at ingress  and egress. This capability gives more flexibility with Cisco 8000 as some of the deployment use cases demand egress remarking rather than performing at ingress.

*Ingress QOS Policy functionality:*
-	Packet classification based on,
o	 L3 :-  ACL, DSCP, Precedence, MPLS-EXP
o	L2 :- CoS, DEI
-	Marking / Re-marking: 
o	Marking of Discard-class (for Dual queueing purpose) , traffic-class (for queueing) , qos-group (internal system marking to match at egress side for remarking applications at egress side)
o	Remarking of DSCP/Precedence, MPLS-EXP, CoC, DEI
-	Policing:
o	Traffic rate limiting at ingress side:- 1R2C, 2R3C

*Egress QOS policy functionality:*
-	Egress Queueing: 
o	Shaping, queueing, BWRR, RED, ECN, priority profiles
-	Egress remarking:
o	Remarking of DSCP/Precedence, MPLS-EXP



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

## Life of packet in VOQs: Enqueuing and Dequeuing
VOQs act as pit stop for packets while traffic manager facilitate packet transport from ingress NPU to egress NPU. VOQs are bound with multiple functions like, enqueue and deque of packets, drop packets at the tail of the queue while over flow caused by congestion,  account the drops in respective VOQs , facilitate ECN marking while congestion experience etc.. 

Lets look at below depiction which briefs packet enqueuing into VOQs:

![enQ.png]({{site.baseurl}}/images/enQ.png)

- System has multiple slices, slice-0 & 2 are receiving traffic on port-1 & 3
- System resolves forwarding to egress port-7 & finds out the corresponding VOQ-ID to enqueue packets into
- Packets enqueues into the VOQ and ingress scheduler get initiated
- Ingress scheduler sends credit request message to egress scheduler 
- Scheduler does book keep of the credit allocation and its consumption etc.
- Egress scheduler is programmed with the egress QOS policy applied on port-7 and has the complete visibility of the traffic situations in each of the OQs associated with port-7: what is the scheduling algorithm applied to each of the output queues , which output queue is congested , what is the situation of port level congestion etc..
- Egress scheduler reply back to ingress scheduler with credits accordingly

Till now traffic is held in VOQs and lets understand further flow from below picture:

![enQ.png]({{site.baseurl}}/images/enQ.png)


- Ingress scheduler dequeue packets out of VOQs as per the credit grant received from egress side
- Dequeued packets will be passed to the respective output queue at TxPP side and transmitted out to wire from there
- If the credit grant is not sufficient then packet start pile up in VOQs at ingress side and lead to congestion after the VOQ capacity exhaust. This can arise when congestion is experienced at egress side
- Queue limit come into apply after VOQ capacity exhaust. Queue limit size is derived from user configured QOS policy at egress side or default system queue limit if there is no explicit queue limit configured
- Queue limit decides how much of additional data can be accommodated in the VOQ when it get congested before tail drop and data above the queue limit size get tail dropped

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

Scope of VOQ replication on Cisco 8000 is at slice level as briefed in above sections. So  fairness is also at slice level.  What that means is, if traffic is incoming on multiple ports acrosss different slices and destined out of same egress port then packet get scheduled out of each VOQs replicated on each ingress slices towards corresponding egress port OQs  in round robin fashion for a given traffic class. So bandwidth is scheduled equally for all slices for that given queue in congestion scenario keeping fair queueing at slice level.

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


## Queueing and Policing on Bundle Ether interfaces

We have seen in above sections that deep buffering takes place at ingress stage of ASIC pipeline. And buffering is at slice level as VOQ replication scope is at slice level. 

First lets look at queueing fairness across slices before jump into the Bundle port scenario,

![bund-1.png]({{site.baseurl}}/images/bund-1.png)

In above picture, 

- 	Traffic ingress on 3 ingress ports (Port-1, 5, 10) destined out of same egress port (Port-20)
-   Bursty traffic (traffic-class 3) is coming in on 3 ports at ingress and all are on different slices
-   Traffic-class 3 is classified and marked as ‘traffic-class 3’ at ingress on all 3 ingress ports
-  Traffic is destined to the same egress port port-20


Ex:-
*policy-map Egress_queuing_policy*

 *class traffic-class-3*
 
  *shape average percent 4*
  
  *queue-limit 10 ms*
  
!

- Traffic get enqueued into VOQ-3 (traffic -class 3) of port-20 on every ingress slices
- Bustiness nature of traffic causes congestion at egress port which forces VOQ-3 at ingress side congested , hence evict to HBM
- Since fairness is at slice level , VOQ-3 of port-20 present in 3 ingress slices get congested and deep buffered as per the queue limit configured 
- VOQ-3 in all 3 ingress slices get 10ms worth of queue pipe in HBM for buffering packets
- With that, total of 30ms worth of queue depth get applied for VOQ-3 of port-20 during congestion
- If all 3 ingress ports are on same slice then aggregate queue depth applicable to VOQ-3 will be 10ms as all 3 ingress source ports enqueue packets into the same VOQ-3


### Queueing on Bundle Ether interface:

![bundle-2.png]({{site.baseurl}}/images/bundle-2.png)

- Traffic flow ingress on one port and destined out of one egress Bundle Ether interface having 3 member links 
- Same Shape rate get programmed on all member links of a given bundle
- Say, ‘shape rate 10gbps’ is the configured shaper in a policy for a given class-map
- Aggregate shape rate on above bundle interface is 30Gbps (10Gbps x 3 member links ) after the queueing policy is applied on the bundle interface
- Flows at ingress get load balanced based on the hash calculated by the load balancing module. 
- So each flow can get maximum of 10Gbps in above example

![bund-3.png]({{site.baseurl}}/images/bund-3.png)

Refer above picture,
- Multiple flows ingress the system which are destined out of egress Bundle ether port 100
- Flows get load balanced as per hash value calculated for each flows
- Egress shaper/queueing policy applied on the bundle ether port get replicated on each members
- Each member can get same shape rate & queue depth if any congestion at egress side


Ex:-
- Say, ‘shape rate 10gbps’ is the configured shaper in a policy for a given class-map with ‘queue limit’ of 10ms which is applied on the bundle ether port 100
- Each member link get programmed with same shape rate & queue limit (10gbps+10ms)
- Say, 3 unique flows are ingressing 
- And 3 flows are destined out of Bundle Ether (BE) port 100 at egress side
- BE-100 has 3x 100G members. (P-1,2,4)
- If there is congestion on any of the member then corresponding VOQ of the member link get congested and evicted to HBM and get maximum queue depth of 10ms


With this, provisioning of QOS applications is easy to deploy on bundle Ether ports where more links can be provisioned as needed as part of capacity expansion. And QOS policy applied on the bundle ether port get programmed on the links which get provisioned newly as and when new links -are added to the bundle ether port.

## Policing 

Policing is more aggressive way of rate limiting and shaping is more controlled way of rate limiting. Major difference between policing and shaping is that policing will drop exceeding traffic and shaping will buffer it.

The logic behind policing is completely different than shaping. To check if traffic matches the traffic contract, the policer will measure the cumulative byte rate of arriving packets conform to certain rate by limiting at that rate. But shaping makes traffic conform to certain rate by delaying packet sending rate. So traffic policing does not cause delay to the traffic flow but shaping causes delay in traffic flow.

Policing can kill the flow which goes slightly over conform rate but shaping can still deliver the spike with little delay. Shaping behaves more like interfaces that have queues to buffer bursts, policing behaves more like interfaces with no queues.

### Policing on Bundle Ether interface:

Cisco 8000 supports policing at ingress direction currently. Scope of policer token bucket programming is at slice level. That means user configured policer on a Bundle interface get programmed at slice level across all member links and not replicated on each member links. Current supported configuration option is in percentage on Bundle main & sub interfaces where absolute and percentage configuration is supported on usual physical main and sub interface types.

![pol-1.png]({{site.baseurl}}/images/pol-1.png)


As shown in above picture, 
- Bundle interface has 3x 100G member links. 2 links are from slice-1 & 1 link form slice-2
- User has configured 50% police rate 
- 50% of total Bundle bandwidth will be the police rate is configured on Bundle Ether interface
- So total police rate applied on the Bundle interface is 50% of 3x100G ie, 150G 
- In other way, since policer replication is at slice level each slice will get 50% of the aggregate bandwidth of all member links on that slice. So slice-1 will be programmed with 100G (50% of 2x 100G) and slice-2 with 50G (50% of 1x 100G)
- With that maximum policed traffic out of Bundle interface in above case is 150G




### Bundle with member links from same slice: 

![pol-2.png]({{site.baseurl}}/images/pol-2.png)

In this example,
- Bundle Ether has 2x 100G member links from same slice: slice-1
- 50% police rate is applied on the Bundle interface
- Slice-1 get programmed with police rate of 100G (50% of 2 x 100G)


Lets look at the traffic flows and the rate limiting,

![pol-3.png]({{site.baseurl}}/images/pol-3.png)


- 2 different flows Flow-1 & Flow-2 ingress the system and both flows are resolved to egress out of the Bundle Ether interface
- Both flows are coming in at 90G rate
- Flows are load balanced as per Bundle hashing  and say, Flow-1 resolved over member link-1 and Flow-2  over link-2

*Traffic generator stats:
![tgn-1.png]({{site.baseurl}}/images/tgn-1.png)


- Rate limiting happens at slice level and not at individual member link level. So each flow gets 50G each totalling to 100G aggregated police rate out of the Bundle Ether port as seen in above traffic generator snap shot.

Let’s see the rate limiting after stopping Flow-2,

*Traffic generator stats:
![tgn-2.png]({{site.baseurl}}/images/tgn-2.png)

- Flow-1 get full throughput as its rate is within the police rate on that slice. With this its evident  that policer replication is at slice level and individual flow can get maximum CIR.

### Bundle with member links from different slices: 

![pol-6.png]({{site.baseurl}}/images/pol-6.png)

In this example,
- Bundle Ether has 2x 100G member links from different slices: slice-1, slice-2
- 50% police rate is applied on the Bundle interface
- Each Slice get programmed with a police rate of 50G (50% of 1 x 100G)
- So total police rate out of Bundle Ether port is 100G


Lets look at the traffic flows and the rate limiting,

![pol-5.png]({{site.baseurl}}/images/pol-5.png)

- 2 different flows Flow-1 & Flow-2 ingress the system and both flows are resolved to egress out of the Bundle Ether interface
- Both flows are coming in at 90G rate
- Flows are load balanced as per Bundle hashing  and say, Flow-1 resolved over member link-1 and Flow-2  over link-2


*Traffic generator stats:
![tgn-1.png]({{site.baseurl}}/images/tgn-1.png)

- Rate limiting happens at slice level and not at individual member link level. So each flow get rate limited to 50G as slice level policing is at 50G and totalling to 100G out of the Bundle Ether port. Traffic generator snapshot given above.

Let’s see the rate limiting after stopping one of the Flow, say Flow-2,

*Traffic generator stats:
![tgn-4.png]({{site.baseurl}}/images/tgn-4.png)

- Flow-2 continues to be rate limited at 50G CIR as policer programming is at slice level. So each Flow get applied with 50G CIR.

## Conclusion
This document briefs fundamental architecture of Cisco 8000 series routers powered by SiOne NPU co-packaged with HBM. QOS architecture explained in this document is applicable to all SiOne NPU generations except latest generation SiOne K100, P100 NPUs which has some of the additional capabilities like Egress Feature Capability(EFC aka eTM) , Priority propagation features etc.And these changes will be documented in upcoming write ups.
