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






















