---
published: true
date: '2025-03-23 14:14 -0700'
title: >-
  Cisco 8000 Line Cards 88-LC1-12TH24FH-E and 88-LC1-52Y8H-EM Architecture White
  Paper
tags:
  - iosxr
  - Silicon One P100
  - Cisco 8000
author: Alexey Babaytsev
excerpt: >-
  This post will describe Cisco 8000 Line Cards 88-LC1-12TH24FH-E and
  88-LC1-52Y8H-EM architecture.
position: hidden
---
{% include toc icon="table" title="Cisco 8000 Line Cards 88-LC1-12TH24FH-E and 88-LC1-52Y8H-EM Architecture White Paper" %}

## Introduction
The [Cisco 8000 Series Routers](https://www.cisco.com/site/us/en/products/networking/routers/8000-series/index.html) represent a significant advancement in high-performance networking, driven by the innovative [Cisco Silicon One™](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html) processor and the robust Cisco IOS XR software. Among the latest additions to this series are the dense 100/400GE - 12T (88-LC1-12TH24FH-E) and low speed combo - 3.7T (88-LC1-52Y8H-EM) line cards, which are designed to meet the evolving demands of modern network infrastructures. These line cards leverage the cutting-edge P100 silicon chip technology, offering exceptional throughput and flexibility. The 88-LC1-12TH24FH-E line card provides an impressive 12 Tbps capacity with a versatile mix of QSFP28-DD and QSFP56-DD ports, while the 88-LC1-52Y8H-EM line card delivers 3.7 Tbps capacity with a diverse array of SFP28, QSFP28, and QSFP56-DD ports, all with MACsec support. This white paper explores the features, benefits, and use cases of these line cards, highlighting their role in enhancing network performance, scalability, and security.

## Line Cards Overview 

### 88-LC1-12TH24FH-E
The Cisco 88-LC1-12TH24FH-E 12 Tbps line card is a recent addition to the Cisco 8000 line card family, based on a P100 ASIC. Featuring 24 ports of QSFP56-DD and 12 ports of QSFP28-DD on the faceplate, the Cisco 88-LC1-12TH24FH-E line card incorporates 4 P100 Silicon One network processors, optimizing the delivery of L2 and L3 services while enhancing the capabilities available in the Q200 based line card portfolio.  

Cisco 88-LC1-12TH24FH-E line card addresses scaled and capacity requirements for Collapsed core, Aggregation, Peering, DC Core, 3rd party colocation, and RON use cases.

![Figure1.png]({{site.baseurl}}/images/Figure1.png)
Figure 1. Front panel view of the 88-LC1-12TH24FH-E line card 
{: .text-center}

In the table below, a high-level Cisco 88-LC1-12TH24FH-E overview is listed.

| Item                | Description                                                                                                                                              |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data Plane          | Four Cisco Silicon One P100 ASICs (7nm)                                                                                                                  |
| Control Plane       | 64 GB DRAM / 256 GB SSD                                                                                                                                  |
| Port Configuration  | 12x QSFP28-DD + 24x QSFP56-DD                                                                                                                            |
| Capabilities        | Egress capabilities enabled on all ports <br>PTP timing with Class C Performance <br>Routed Optical Networking Architecture Ready (Supports DCO optics)  |
| Software            | Supports Cisco IOS XR Software Release 24.3.1 and later                                                                                                  |
| Typical Power       | 1410W w/o Optics                                                                                                                                         |
| Commons             | Needs 8800-RP2 & 8808-FC1 or 8804-FC1 Fabric cards <br>5 fabric cards for N + 1 redundancy                                                               |
| Use Case            | Collapsed Core, Aggregation, Peering, DC Core, Enterprise WAN Edge                                                                                       |

Table 1. Cisco 88-LC1-12TH24FH-E key characteristics 
{: .text-center}

### 88-LC1-52Y8H-EM
Cisco 88-LC1-52Y8H-EM 3.7 Tbps line card is built based on 2 P100 ASICs. With 4 ports of QSFP56-DD, 8 ports of QSFP28, and 52 ports of SFP28 on the faceplate, and support of MACsec on all ports, Cisco 88-LC1-12TH24FH-E line card addresses scaled and capacity requirements for Collapsed core, Aggregation, Peering, DC Core, 3rd party colocation, and RON use cases. 

![Figure2.png]({{site.baseurl}}/images/Figure2.png)
Figure 2. Front panel view of the 88-LC1-52Y8H-EM line card
{: .text-center}

In the table below, a high-level Cisco 88-LC1-52Y8H-EM overview is listed.  

| Item                | Description                                                                                                                                                                       |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data Plane          | Two Cisco Silicon One P100 (7nm)                                                                                                                                                  |
| Control Plane       | Intel 6 Core x86 CPU <br>64 GB DRAM / 256 GB SSD                                                                                                                                  |
| Port Configuration  | 52x SFP28 + 8x QSFP28 + 4x QSFP56-DD                                                                                                                                              |
| Capabilities        | Egress capabilities enabled on all ports <br>PTP timing with Class C Performance￼ <br>MACsec on all ports <br>Routed Optical Networking Architecture Ready (Supports DCO optics)  |
| Software            | Supports Cisco IOS XR Software Release 24.3.1 and later                                                                                                                           |
| Typical Power       | 850W w/o Optics                                                                                                                                                                   |
| Commons             | Needs 8800-RP2 & 8808-FC1 or 8804-FC1 Fabric cards <br>5 fabric cards for N + 1 redundancy                                                                                        |
| Use Case            | Collapsed Core, Aggregation, Peering, DC Core, Enterprise WAN Edge                                                                                                                |

Table 2. Cisco 88-LC1-52Y8H-EM key characteristics
{: .text-center}

## Use Cases

### Aggregation, Core, Peering, and DC Core with RON (Routed Optical Networking)
As user applications become increasingly dynamic and demand differentiated experiences, optimizing network resources in terms of power, scale, and flexibility is crucial. The Cisco 8000 line cards, 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E, are designed to meet market demands for high system bandwidth, port diversity, and security. The 88-LC1-52Y8H-EM line card supports MACsec encryption, while both line cards provide enhanced scalability and advanced functionalities such as egress policing, queuing, flowspec, and support for a variety of other features for service providers, data centers, and enterprise customers. These features empower the Cisco 8000 to be utilized in various scenarios, including collapsed core, L3 gateway, aggregation and peering, and enterprise WAN edge. 

![Figure3.png]({{site.baseurl}}/images/Figure3.png)
Figure 3. Aggregation, Core, Peering and DC Core with RON Use Case
{: .text-center}

Cisco’s [Converged SDN Transport Solution](https://blogs.cisco.com/sp/routed-optical-networking-its-about-the-architecture), is an architecture that delivers efficient network utilization, Simplified single layer, reduced network complexity, faster time to market, automation empowerment, and differentiated service offering. The solution works by merging IP and Optical onto a single layer where all the switching is done at Layer 3. Routers are connected with standardized [400G ZR/ZR+/Bright(0dBm) ZR+ coherent pluggable optics](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/24xx/configuration/guide/b-interfaces-config-guide-cisco8k-r24xx/m-zr-zrp-cisco-8000.html). With a single service layer based upon IP, flexible management tools can leverage [telemetry](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/telemetry/24xx/configuration/guide/b-telemetry-cg-8000-24xx.html) and [model-driven programmability](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/programmability/24xx/configuration/guide/b-programmability-cg-8000-24xx.html) to streamline lifecycle operations. 

### Secure High-speed Data Center/Cloud interconnect
![Figure4.png]({{site.baseurl}}/images/Figure4.png)
Figure 4. Secure High-speed Data Center/Cloud Interconnect Use Case 
{: .text-center}

The requirements for securing the connection of multiple data-center and cloud links typically target a smaller number of links, while requiring the highest level of bandwidth requirements. Sometimes called data center interconnect (DCI), WAN MACsec is an ideal encryption solution for interconnecting data centers or multilocation cloud environments. 

The backhauling of remote branch sites for government, enterprise, or commercial organizations is critical to any business. In the consumer space, this can be important for remote stores and point-of-sale kiosks, and in the enterprise and government space, it is crucial for remote agencies and offices.

### Collapsed Core Use Case
In contemporary networks, numerous customers opt to integrate multiple functionalities within a single location and device. The collapsed core design facilitates the coexistence of traditional core/peering features alongside aggregation/edge functionality, with improved L2/L3 service termination. The Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E line cards support popular core and edge features such as MPLS, SR, SRTE, SRv6, L3VPN, L2VPN, QoS, OAM, and EVPN. 

![Figure5.png]({{site.baseurl}}/images/Figure5.png)
Figure 5. Collapsed Core Use Case
{: .text-center}

## Understanding the Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E Naming Logic 

![Figure6.png]({{site.baseurl}}/images/Figure6.png)
Figure 6. Cisco 88-LC1-52Y8H-EM Naming 
{: .text-center} 

![Figure7.png]({{site.baseurl}}/images/Figure7.png)
Figure 7. Cisco 88-LC1-52Y8H-EM Naming
{: .text-center} 

The complete list of PIDs related to Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E are below. 

| PIDs               | Product Description                                         |
|--------------------|-------------------------------------------------------------|
| 88-LC1-12TH24FH-E  | Cisco 8800 12x QSFP28-DD, 24x QSFP56-DD; 12 Tbps line card  |
| 8KSW-C-SIA-3       | 8000 Type C Device SIA for 3 to 5 year term FCM 2.0         |
| 8KSW-C-SIA-5       | 8000 Type C Device SIA for 5 to 7 year term FCM 2.0         |
| ESS-8KE-100G-RTU   | Essentials Right-to-Use 100G for Cisco 8000 Series          |
| ADN-8KE-100G-RTU   | Advantage Right-to-Use 100G for Cisco 8000 Series           |
| PRM-8KE-100G-RTU   | Premium Right-to-Use 100G for Cisco 8000 Series             |

Table 3. 88-LC1-12TH24FH-E
{: .text-center}

| PIDs              | Product Description                                                |
|-------------------|--------------------------------------------------------------------|
| 88-LC1-52Y8H-EM   | Cisco 8800 52x SFP28, 8x QSFP28, 4x QSFP56-DD, 3.7 Tbps line card  |
| 8KSW-B-SIA-3      | 8000 Type B Device SIA for 3 to 5 year term FCM 2.0                |
| 8KSW-B-SIA-5      | 8000 Type B Device SIA for 5 to 7 year term FCM 2.0                |
| ESS-8KE-100G-RTU  | Essentials Right-to-Use 100G for Cisco 8000 Series                 |
| ADN-8KE-100G-RTU  | Advantage Right-to-Use 100G for Cisco 8000 Series                  |
| PRM-8KE-100G-RTU  | Premium Right-to-Use 100G for Cisco 8000 Series                    |

Table 4. Cisco 88-LC1-52Y8H-EM
{: .text-center}

## Cisco 88-LC1-12TH24FH-E and 88-LC1-52Y8H-EM Faceplate Design and Port Numbering

![Figure8.png]({{site.baseurl}}/images/Figure8.png)
Figure 8. Cisco 88-LC1-12TH24FH-E port numbering
{: .text-center} 


![Figure9.png]({{site.baseurl}}/images/Figure9.png)
Figure 9. Cisco 88-LC1-52Y8H-EM port numbering
{: .text-center} 

Cisco 88-LC1-12TH24FH-E and 88-LC1-52Y8H-EM port numbering in the system is denoted as {Type}{R/S/I/P} or {Type}{R/S/I/P/B}

![Figure-port-numbering.png]({{site.baseurl}}/images/Figure-port-numbering.png)


Native 400GbE Interface (Cisco 88-LC1-12TH24FH-E as an example) 

<div class="highlighter-rouge">
<pre class="highlight">
<code>  
 
RP/0/RP0/CPU0:8808#<span style="background-color: #A0CFEC">show interfaces brief | i 0/7/0/32</span>
Interface                      IP-Address      Status          Protocol Vrf-Name 
FourHundredGigE0/7/0/32        unassigned      Down            Down     default 

</code>
</pre>
</div>  
 
Breakout Interface Use-case with 4x100 GbE at 400G port (Cisco 88-LC1-12TH24FH-E as an example) 

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

controller Optics0/7/0/33 
 breakout <span style="background-color: #00FF00">4x100</span>
! 

RP/0/RP0/CPU0:8808#<span style="background-color: #A0CFEC">show interfaces brief | i 0/7/0/33</span>
Interface                      IP-Address      Status          Protocol Vrf-Name 
HundredGigE0/7/0/33/0          unassigned      Shutdown        Down     default 
HundredGigE0/7/0/33/1          unassigned      Shutdown        Down     default 
HundredGigE0/7/0/33/2          unassigned      Shutdown        Down     default 
HundredGigE0/7/0/33/3          unassigned      Shutdown        Down     default 

</code>
</pre>
</div>  

## System Details 

### System Block Diagram

The Cisco 88-LC1-12TH24FH-E line card is built based on four P100 NPU processors with aggregated bandwidth of 12 Tbps. The line card has two types of faceplate ports on the network side – QSFP56-DD 400G (native) or QSFP28-DD 200G (2x100G breakout) ports. Other breakout interface options supported are – 4x10G, 4x25G. Fabric interfaces connect the line card with the fabric cards of the chassis. The block diagram of Cisco 88-LC1-12TH24FH-E is presented in Figure 10.  

![Figure10.png]({{site.baseurl}}/images/Figure10.png)
Figure 10. Cisco 88-LC1-12TH24FH-E Block Diagram
{: .text-center} 

The Cisco 88-LC1-52Y8H-EM line card shares a similar design approach with the Cisco 88-LC1-12TH24FH-E, both utilizing P100 NPU processors. However, the 88-LC1-52Y8H-EM features two NPUs, providing an aggregated bandwidth of 3.7 Tbps. This line card supports a variety of native interface speeds, including 400G, 100G, 40G, 25G, and 10G. A key feature of the Cisco 88-LC1-52Y8H-EM line card is its MACsec capability, enabled by the use of an additional PHY ASIC. 

The block diagram of Cisco 88-LC1-52Y8H-EM is presented in Figure 11.  

![Figure11.png]({{site.baseurl}}/images/Figure11.png)
Figure 11. Cisco 88-LC1-52Y8H-EM Block Diagram
{: .text-center}

### P100 NPU Overview

The P100 family comprises high-capacity packet switching devices, each consisting of six slices. Every slice includes a receive and a transmit component (RX slice and TX slice) and supports two groups of interfaces (a pair of IFGs). In terms of performance, each slice can process up to one packet per clock cycle and offers a data bandwidth of up to 3.2 Tbps. The total capacity of a P100 device is determined by the number of slices utilized for a specific product type. Both the Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E line cards employ three slices on the network side and three slices on the fabric side.  

Cisco Silicon One P100 features: 
- 19.2 Tbps full-duplex and 8.1 Bpps Forwarding Capacity NPU
  - 192x 100G SerDes with each capable of operating at 10G/25G/50G/100G using NRZ or PAM4 modulation 
  - Flexible port configuration supporting 10/25/40/50/100/200/400/800 Gbps 
  - 72 MB fully shared on-die packet buffer
- Expandable in-package packet buffer using 8 GB HBM 
- On-chip, high-performance, P4-programmable host NPU for high-bandwidth offline packet processing 
- Multiple embedded processors for CPU offloading 
- 64K VoQ(Virtual Output Queue) for Network slice and 16K VoQ for Fabric slice 
- Dedicated 132 Counters banks (1M counters) and built-in counters in HCAM 
- 6M IPv4 or 3M IPv6 FIB qualified scale without compression (Hardware capable much more)

![Figure12.png]({{site.baseurl}}/images/Figure12.png){: .align-center} 

Figure 12. Cisco Silicon One P100
{: .text-center}

Cisco 8000 P100-based line cards use 3 slices of each P100 ASIC as network-facing interfaces and 3 slices as fabric-facing, optimizing for high feature scale and high capacity when it’s used in the distributed system. 

![Figure13.png]({{site.baseurl}}/images/Figure13.png){: .align-center} 

Figure 13. Cisco Silicon One P100 architecture used in Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E line cards
{: .text-center}

For more details on Cisco Silicon One P100, refer to [Cisco Silicon One P100 Processor Data Sheet](https://www.cisco.com/c/en/us/solutions/collateral/silicon-one/silicon-one-p100-processor-ds.html). 

### Port/IFG/Slice assignment

When customers contemplate upgrading from legacy equipment to the new Cisco 8000 Routers, gaining insight into the internal architecture of Cisco 8000 line cards is essential. The processing resources of the Cisco Silicon One ASIC are distributed among different internal components, including slices and interface groups (IFGs). Understanding the allocation of physical ports to NPU/slice/IFG is vital for the migration and new deployment planning. The exact port mapping information can be derived by using the following CLI: 

<div class="highlighter-rouge">
<pre class="highlight">
<code>  
show controllers npu voq-usage interface all instance all location all 
</code>
</pre>
</div>  

The consolidated physical port to NPU/Slice/IFG mapping information of line cards Cisco 88-LC1-12TH24FH-E and 88-LC1-52Y8H-EM is below.

| Interface                    | NPU/Slice/IFG  | Interface                    | NPU/Slice/IFG  | Interface                     | NPU/Slice/IFG  |
|------------------------------|----------------|------------------------------|----------------|-------------------------------|----------------|
| Hu0/7/0/6/1 <br>FH0/7/0/9    | 0/0/0          | FH0/7/0/15 <br>Hu0/7/0/12/1  | 1/1/0          | FH0/7/0/21 <br>Hu0/7/0/18/1   | 2/2/0          |
| FH0/7/0/7 <br>Hu0/7/0/6/0    | 0/0/1          | FH0/7/0/13 <br>Hu0/7/0/12/0  | 1/1/1          | FH0/7/0/19 <br>Hu0/7/0/18/0   | 2/2/1          |
| FH0/7/0/5 Hu0/7/0/4/1        | 0/1/0          | FH0/7/0/11 <br>Hu0/7/0/10/1  | 1/2/0          | FH0/7/0/29 <br>Hu0/7/0/28/1   | 3/0/0          |
| FH0/7/0/3 <br>Hu0/7/0/4/0    | 0/1/1          | FH0/7/0/8 <br>Hu0/7/0/10/0   | 1/2/1          | FH0/7/0/27 <br> Hu0/7/0/28/0  | 3/0/1          |
| FH0/7/0/1 <br>Hu0/7/0/0/0    | 0/2/0          | FH0/7/0/35 <br>Hu0/7/0/34/1  | 2/0/0          | FH0/7/0/26 <br>Hu0/7/0/24/1   | 3/1/0          |
| FH0/7/0/2 <br>Hu0/7/0/0/1    | 0/2/1          | FH0/7/0/33 <br>Hu0/7/0/34/0  | 2/0/1          | FH0/7/0/25 <br>Hu0/7/0/24/0   | 3/1/1          |
| FH0/7/0/14 <br>Hu0/7/0/16/1  | 1/0/0          | FH0/7/0/32 <br>Hu0/7/0/30/1  | 2/1/0          | FH0/7/0/23 <br>Hu0/7/0/22/0   | 3/2/0          |
| FH0/7/0/17 <br>Hu0/7/0/16/0  | 1/0/1          | FH0/7/0/31 <br>Hu0/7/0/30/0  | 2/1/1          | FH0/7/0/20 <br>Hu0/7/0/22/1   | 3/2/1          |

Table 5. Port-IFG-Slice mapping of Cisco 88-LC1-12TH24FH-E
{: .text-center}

| Interface                                                                                                        | NPU/Slice/IFG  | Interface                                                                                                                                      | NPU/Slice/IFG  | Interface                                                                                                                                          | NPU/Slice/IFG  | Interface                                                                                                            | NPU/Slice/IFG  |
|------------------------------------------------------------------------------------------------------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------------|----------------------------------------------------------------------------------------------------------------------|----------------|
| FH0/2/0/31                                                                                                       | 0/0/0          | TF0/2/0/7 <br>TF0/2/0/8 <br>TF0/2/0/10 <br>TF0/2/0/11 <br>TF0/2/0/13 <br>TF0/2/0/14 <br>TF0/2/0/16 <br>TF0/2/0/17                              | 0/1/1          | TF0/2/0/61 <br>TF0/2/0/58 <br>TF0/2/0/55 <br>TF0/2/0/52 <br>TF0/2/0/49 <br>TF0/2/0/34 <br>TF0/2/0/37 <br>TF0/2/0/40 <br>TF0/2/0/43 <br>TF0/2/0/46  | 1/0/0          | TF0/2/0/35 <br>TF0/2/0/36 <br>TF0/2/0/38 <br>TF0/2/0/39 <br>TF0/2/0/41 <br>TF0/2/0/42 <br>TF0/2/0/44 <br>TF0/2/0/45  | 1/1/1          |
| FH0/2/0/30                                                                                                       | 0/0/1          | Hu0/2/0/5 <br>Hu0/2/0/4 <br>Hu0/2/0/2 <br>Hu0/2/0/1                                                                                            | 0/2/0          | Hu0/2/0/63 <br>Hu0/2/0/59 <br>Hu0/2/0/60 <br>Hu0/2/0/62                                                                                            | 1/0/1          | FH0/2/0/33                                                                                                           | 1/2/0          |
| TF0/2/0/19 <br>TF0/2/0/20 <br>TF0/2/0/22 <br>TF0/2/0/23 <br>TF0/2/0/25 <br>TF0/2/0/26 <br>TF0/2/0/28 <br>TF0/2/0/29  | 0/1/0          | TF0/2/0/0 <br>TF0/2/0/3 <br>TF0/2/0/6 <br>TF0/2/0/9 <br>TF0/2/0/12 <br>TF0/2/0/15 <br>TF0/2/0/18 <br>TF0/2/0/21 <br>TF0/2/0/24 <br>TF0/2/0/27  | 0/2/1          | TF0/2/0/57 <br>TF0/2/0/56 <br>TF0/2/0/54 <br>TF0/2/0/53 <br>TF0/2/0/51 <br>TF0/2/0/50 <br>TF0/2/0/48 <br>TF0/2/0/47                                | 1/1/0          | FH0/2/0/32                                                                                                           | 1/2/1          |

Table 6. Port-IFG-Slice mapping of Cisco 88-LC1-52Y8H-EM
{: .text-center}

## Port Options

### Maximum port scale and support on Cisco 88-LC1-12TH24FH-E 
The following table represents the maximum number of ports the Cisco 88-LC1-12TH24FH-E line card can support. 

|   Speed   |    10 GbE     | 25 GbE  | 40 GbE  | 100 GbE  | 400 GbE  |
|:---------:|:-------------:|:-------:|:-------:|:--------:|:--------:|
|  Native   | 36 (Roadmap)  |   N/A   |   18    |    36    |    24    |
| Breakout  |      72       |   144   |   N/A   |   120    |   N/A    |

10 GbE 
- As Native 10G Optic with CVR-QSFP-SFP10G (QSA), fully populated into all 36 Ports in roadmap 
- As Breakout, populated into odd ports. 18 ports x (4x10 GbE) = 72x10 GbE total

25 GbE 
- As Breakout, fully populated into all 36 Ports. 36 ports x (4x25 GbE) = 144x25 GbE total

40 GbE 
- As Native 40G Optic, populated into odd ports, 18x40GbE total.

100 GbE 
- As Native 100G Optic, fully populated into all 36 Ports  
- As 2x100 GbE breakout populated into 12 QSFP28-DD ports, as 4x100 GbE breakout populated into 24 QSFP56-DD ports = 120 x 100 GbE total

400 GbE 
- As Native 400G Optic, fully populated into 24 QSFP56-DD Ports

### Maximum port scale and support on Cisco 88-LC1-52Y8H-EM 
The following table represents the maximum number of ports the 88-LC1-52Y8H-EM line card can support. 

|   Speed   | 10 GbE  | 25 GbE  | 40 GbE  | 100 GbE  | 400 GbE  |
|:---------:|:-------:|:-------:|:-------:|:--------:|:--------:|
|  Native   |   52    |   52    |   12    |    12    |    4     |
| Breakout  |   48    |   48    |   N/A   |    16    |   N/A    |

10 GbE 
- As Native 10G Optic, fully populated into all 52 SFP28 Ports.  
- As Breakout, fully populated into all QSFP28 ports and QSFP56-DD ports = 12 ports x (4x10 GbE) = 48x10 GbE total.

25 GbE 
- As Native 25G Optic, fully populated into all 52 SFP28 Ports. 
- As Breakout, fully populated into all QSFP28 ports and QSFP56-DD ports = 12 ports x (4x25 GbE) = 48x25 GbE total.

40 GbE 
- As Native 40G Optic, fully populated into all QSFP28 ports and QSFP56-DD ports. 

100 GbE 
- As Native 100G Optic, fully populated into 12 QSFP Ports  
- As 4x100 GbE breakout populated into 4 QSFP56-DD ports = 16 x 100 GbE total.

400 GbE 
- As Native 400G Optic, fully populated into 4 QSFP56-DD Ports

<b>Other things to know?</b>

- 10 GbE optics will be supported via QSFP to SFP Adaptor (QSA: CVR-QSFP-SFP10G) on Cisco 88-LC1-12TH24FH-E (roadmap). 
- There is no plan to support 10 GbE optics via QSFP to SFP Adaptor on Cisco 88-LC1-52Y8H-EM. Native SFP28 ports can be used instead.  
- 1GbE optic will be supported on 88-LC1-52Y8H-EM (roadmap).

For more information on Cisco transceiver modules supported on Cisco 8000 products, refer to [Cisco Optics-to-Device Compatibility Matrix](https://tmgmatrix.cisco.com/). 

## Egress Feature Capability support on Cisco 88-LC1-12TH24FH-E and Cisco 88-LC1-52Y8H-EM Line Cards 
The Cisco 8000 P100-based line cards, specifically the Cisco 88-LC1-12TH24FH-E and Cisco 88-LC1-52Y8H-EM, offer advanced egress feature capabilities designed to enhance network performance and efficiency. These line cards support multicast traffic scheduling on egress queues, allowing for granular control over multicast traffic. This feature enables the application of egress queuing policy map parameters such as traffic shaping, priority, and queuing to each egress queue, ensuring efficient and prioritized handling of multicast data for different receivers. 

Additionally, the line cards provide egress class-level traffic shaping for subinterfaces, allowing for optimal bandwidth allocation and improved network performance. This is achieved by enabling class-level rate-limiting traffic shapers for each class in the egress subinterface-level queuing policy maps. This feature ensures that traffic flow is controlled granularly, enhancing overall network efficiency. 

Furthermore, the priority propagation on the egress queues feature ensures that high-priority traffic is consistently prioritized across multiple sub-interfaces on the same network port. This capability enhances the performance of critical applications by allowing high-priority traffic to take precedence over non-priority traffic, maintaining optimal network performance, and reducing latency for essential services such as voice and real-time video. These egress features collectively contribute to the robust and efficient operation of networks utilizing the Cisco 8000 P100-based line cards. 

## MACsec support on Cisco 88-LC1-52Y8H-EM  
Starting IOS-XR 24.3.1, MACsec is supported on all ports of Cisco 88-LC1-52Y8H-EM including breakout ports. 

- Full line-rate MACsec encryption engines to enable secure and low-latency transport 
- Supports MACsec GCM-AES-128/256 and GCM-AES-XPN-128/256 
- Supports LAN MACsec and WAN MACsec 
- Supports “VLAN tag in the clear”.

**Note**: k9 package is required for crypto data plane.
{: .notice--info}

Cisco 88-LC1-52Y8H-EM can support the following features to interoperate with the service provider. 

- Ability to change MKA EAPoL Destination Address type 
- Ability to change MKA Ether-type value 
- Ability to configure Anti-replay window sizes

Cisco 88-LC1-52Y8H-EM can support the following MACsec Use Cases.

![Figure-macsec.png]({{site.baseurl}}/images/Figure-macsec.png)

For more information on MACsec configuration and features supported on Cisco 8000 products, refer to [MACsec configurations on Cisco 88-LC1-52Y8H-EM](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/security/24xx/configuration/guide/b-system-security-cg-cisco8000-24xx/configuring-macsec.html).

## Conclusion 
This document outlines the architecture of the P100-based Cisco 88-LC1-12TH24FH-E and Cisco 88-LC1-52Y8H-EM line cards. These line cards provide high port density for 10G, 25G, 100G, and 400G, including support for coherent optics in RON-based architecture. The Cisco 88-LC1-52Y8H-EM is particularly beneficial for low-latency secure solutions with MACsec support. Both the Cisco 88-LC1-12TH24FH-E and Cisco 88-LC1-52Y8H-EM, with their enhanced egress capabilities, are designed to serve as routing solutions for next-generation core, aggregation, peering, and data center core networks. 

## References
[Cisco Silicon One™](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html)

[Cisco 8800 Series P100-Based Line Cards Data Sheet](https://www.cisco.com/c/en/us/products/collateral/routers/8000-series-routers/8000-series-p100-line-card-ds.html)

[Hardware Installation Guide for Cisco 8800 Series Routers](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/hardware/hig-modular/b-8800-hardware-installation-guide-modular.html)

[Cisco 8000 Configuration guide](https://www.cisco.com/c/en/us/support/routers/8000-series-routers/products-installation-and-configuration-guides-list.html)

[Cisco Optics-to-Device Compatibility Matrix tool](https://tmgmatrix.cisco.com/)

[Cisco 8000 Power Calculator](https://8000-power.cisco.com/)



