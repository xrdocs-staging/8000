---
published: false
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
---
{% include toc icon="table" title="Cisco 8000 Line Cards 88-LC1-12TH24FH-E and 88-LC1-52Y8H-EM Architecture White Paper" %}

## Introduction
The [Cisco 8000 Series Routers](https://www.cisco.com/site/us/en/products/networking/routers/8000-series/index.html) represent a significant advancement in high-performance networking, driven by the innovative [Cisco Silicon One™](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html) processor and the robust Cisco IOS XR software. Among the latest additions to this series are the dense 100/400GE - 12T (88-LC1-12TH24FH-E) and low speed combo - 3.7T (88-LC1-52Y8H-EM) line cards, which are designed to meet the evolving demands of modern network infrastructures. These line cards leverage the cutting-edge P100 silicon chip technology, offering exceptional throughput and flexibility. The 88-LC1-12TH24FH-E line card provides an impressive 12 Tbps capacity with a versatile mix of QSFP28-DD and QSFP56-DD ports, while the 88-LC1-52Y8H-EM line card delivers 3.7 Tbps capacity with a diverse array of SFP28, QSFP28, and QSFP56-DD ports, all with MACsec support. This white paper explores the features, benefits, and use cases of these line cards, highlighting their role in enhancing network performance, scalability, and security.

## Line Cards Overview 

## 88-LC1-12TH24FH-E
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

## 88-LC1-52Y8H-EM 
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

## Aggregation, Core, Peering, and DC Core with RON (Routed Optical Networking) 
As user applications become increasingly dynamic and demand differentiated experiences, optimizing network resources in terms of power, scale, and flexibility is crucial. The Cisco 8000 line cards, 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E, are designed to meet market demands for high system bandwidth, port diversity, and security. The 88-LC1-52Y8H-EM line card supports MACsec encryption, while both line cards provide enhanced scalability and advanced functionalities such as egress policing, queuing, flowspec, and support for a variety of other features for service providers, data centers, and enterprise customers. These features empower the Cisco 8000 to be utilized in various scenarios, including collapsed core, L3 gateway, aggregation and peering, and enterprise WAN edge. 

![Figure3.png]({{site.baseurl}}/images/Figure3.png)
Figure 3. Aggregation, Core, Peering and DC Core with RON Use Case
{: .text-center}

Cisco’s [Converged SDN Transport Solution](https://blogs.cisco.com/sp/routed-optical-networking-its-about-the-architecture), is an architecture that delivers efficient network utilization, Simplified single layer, reduced network complexity, faster time to market, automation empowerment, and differentiated service offering. The solution works by merging IP and Optical onto a single layer where all the switching is done at Layer 3. Routers are connected with standardized [400G ZR/ZR+/Bright(0dBm) ZR+ coherent pluggable optics](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/24xx/configuration/guide/b-interfaces-config-guide-cisco8k-r24xx/m-zr-zrp-cisco-8000.html). With a single service layer based upon IP, flexible management tools can leverage [telemetry](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/telemetry/24xx/configuration/guide/b-telemetry-cg-8000-24xx.html) and [model-driven programmability](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/programmability/24xx/configuration/guide/b-programmability-cg-8000-24xx.html) to streamline lifecycle operations. 

## Secure High-speed Data Center/Cloud interconnect
![Figure4.png]({{site.baseurl}}/images/Figure4.png)
Figure 4. Secure High-speed Data Center/Cloud Interconnect Use Case 
{: .text-center}

The requirements for securing the connection of multiple data-center and cloud links typically target a smaller number of links, while requiring the highest level of bandwidth requirements. Sometimes called data center interconnect (DCI), WAN MACsec is an ideal encryption solution for interconnecting data centers or multilocation cloud environments. 

The backhauling of remote branch sites for government, enterprise, or commercial organizations is critical to any business. In the consumer space, this can be important for remote stores and point-of-sale kiosks, and in the enterprise and government space, it is crucial for remote agencies and offices.

## Collapsed Core Use Case 
In contemporary networks, numerous customers opt to integrate multiple functionalities within a single location and device. The collapsed core design facilitates the coexistence of traditional core/peering features alongside aggregation/edge functionality, with improved L2/L3 service termination. The Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E line cards support popular core and edge features such as MPLS, SR, SRTE, SRv6, L3VPN, L2VPN, QoS, OAM, and EVPN. 

![Figure5.png]({{site.baseurl}}/images/Figure5.png)
Figure 5. Collapsed Core Use Case
{: .text-center}

## Understanding the Cisco 88-LC1-52Y8H-EM and 88-LC1-12TH24FH-E Naming Logic 
![Figure6.png]({{site.baseurl}}/images/Figure6.png)
Figure 6. Cisco 88-LC1-52Y8H-EM Naming 
{: .text-center} 