---
published: false
date: '2025-05-03 12:26 +0530'
title: Cisco Silicon One™ based 8712-MOD-M Router Architecture
---
|Ram Mohan A.M., Technical Leader, Technical Marketing Engineering (raam@cisco.com)|  
{% include toc icon="table" title="Cisco Silicon One™ based 8712-MOD-M Router Architecture White Paper" %}



## Introduction
The Cisco 8000 Series combines the revolutionary Cisco Silicon One™, IOS XR® software, and a set of clean sheet chassis to deliver a breakthrough in high-performance routers. 
High-performance networking systems have historically been divided into routing or switching classes, with distinct hardware and software. But, as the necessity for reliable networks proliferates, it is imperative that traditional networks shift to a new architecture to address exponential bandwidth needs, ubiquitous connectivity, security, better quality of service, low latency coupled with high reliability, availability, and serviceability of the infrastructure. The Cisco® 8000 Series portfolio completes this journey with the Cisco 8700 products.
The Cisco 8700 series is an addition to [the Cisco 8000 Series Routers](https://www.cisco.com/site/us/en/products/networking/routers/8000-series/index.html) powered by [Cisco Silicon One](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html)™  ASICs.  8700 products embody this cutting-edge innovation of the 8000 portfolio, offering advanced features engineered for seamless integration and scalability. Whether enhancing existing infrastructure or enabling new capabilities, the 8700 series products empower organizations to achieve their goals with unmatched efficiency and effectiveness. 

## Cisco 8712-MOD-M Overview  
Cisco 8712-MOD-M is a 2RU 6.4Tbps fixed system based on a single Cisco Silicon One™ K100 Network Processing Unkit (NPU). This system offers 4 bays of Modular Port Adaptors (MPAs) with each slot having 1.6Tbps capacity. 4 types of MPAs are supported which offers diverse port speed combinations supported by SFP56, QSF28, QSFPDD optical form factors: 1G, 10G, 25G, 50G, 100G, 400G.

## Video  

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=2ckU9oAP9hI&t=17s" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>  
  
The key features of the Cisco 8712-MOD-M are summarized below.     

![product-specs.png]({{site.baseurl}}/images/product-specs.png)

## Service Provider(SP) networking Architecture shift


Lets look at traditional SP network architecture and the how it is transitioning to nextGen network driven by Agile Services Networking architecture.

Network roles are strictly defined and deployed in networks. Devices must be groomed specifically to address different roles in network. And feature capability , scale and bandwidth requirements are differently fitted to address different needs in the network.

Common view of traditional service provider network is as following, different services like business, wireless, mobile etc. are terminated into the transport aggregation which includes further nailing down to access, preaggregation and aggregation network segments etc.. Aggregated transport networks talk to Edge then merge into the  Core segment. Edge can segregated to cater to different use cases as per the specific need of each categories like, Business edge, Mobile edge, Wireless edge etc..or all types of edge services can be centrally placed and provisioned.


## Cisco 8712-MOD-M Use Cases  
### Aggregation, Core, Peering and DC Core with RON(Routed Optical Networking)       
![use-case.png]({{site.baseurl}}/images/use-case.png)  

**As user applications become more dynamic, differentiated user experiences are required, and network resources need to be optimized in terms of power and real estate. Cisco 8712-MOD-M has been designed to cater to the market requirement for total system bandwidth, port diversity, and MACsec encryption. These new capabilities far exceed what could be accomplished with a single P100 NPU. Specifically, Cisco 8712-MOD-M supports 12.8 Tbps of forwarding capacity, as well as 32 ports of high speed 400G, 128 ports of 100G, or 128 ports of low speed 10G or 25G.  

Moreover, Cisco’s [Converged SDN Transport Solution](https://blogs.cisco.com/sp/routed-optical-networking-its-about-the-architecture), is a simplified single layer architecture that delivers efficient network utilization, reduced network complexity, faster time-to-market, automation empowerment, and differentiated service offering. This solutions works by merging IP and Optimal onto a single layer where all the switching is done. Routers are connected with standardized [400G ZR/ZR+/Bright(0dBm) ZR+ coherent pluggable optics](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/710x/b-interfaces-config-guide-cisco8k-r710x/m-zr-zrp-cisco-8000.html). With a single service layer based upon IP, flexible management tools can leverage [telemetry](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/telemetry/710x/configuration/guide/b-telemetry-cg-8000-710x.html) and [model-driven programmability](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/programmability/710x/b-programmability-cg-8000-710x.html) to streamline lifecycle operations.  

### Secure High-speed Data Center/Cloud interconnect and large Regional Hub/Branch router    
![Screenshot 2024-12-24 at 5.08.50 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-24 at 5.08.50 PM.png)  

Securing the connections of multiple data centers and cloud environments typically involves meeting the highest levels of bandwidth requirements on a smaller number of links. WAN MACsec, sometimes called data center interconnect (DCI), is the ideal encryption solution for such settings.    

Backhauling remote branch sites is critical to the success of many commercial, enterprise, and government organizations. In the consumer space, backhauling is important for remote stores and point-of-sale kiosks. In the government space, it is crucial for the operation of remote agencies and offices.    


*Understand Cisco 8712-MOD-M’s modules and associated PIDs

| PIDs             | Product Description                                           |
|------------------|---------------------------------------------------------------|
| 8712-MOD-M       | Cisco 8712 2RU 6.4T KR100 System with 4 MPA bays              |
| 8000-QSFP-DCAP   | QSFP Dust CAP                                                 |
| 8700-1RU-4P-KIT  | Rack Mount Kit for Cisco 8700 1RU Chassis 19” and 23”, 4 POST |
| 8700-1RU-2P-KIT  | Rack Mount Kit for Cisco 8700 1RU Chassis 19” and 23”, 2 POST |
| FAN-1RU-PI-V2    | 1RU Fan with Port-side Air Intake                             |
| FAN-1RU-PE-V2    | 1RU Fan with Port-side Air Exhaust                            |
| PSU2KW-ACPI      | 2000W AC Power Module with Port-side Air Intake               |
| PSU2KW-ACPE      | 2000W AC Power Module with Port-side Air Exhaust              |
| PSU2KW-DCPI      | 2000W DC Power Module with Port-side Air Intake               |
| PSU2KW-DCPE      | 2000W DC Power Module with Port-side Air Exhaust              |
| PWR-2KW-DC-CBL   | 2000W DC Power Cable                                          |
| ESS-8KE-100G-RTU | Essential Right-to-Use 100G for Cisco 8000E Series            |
| ADN-8KE-100G-RTU | Advantage Right-to-Use 100G for Cisco 8000E Series            |
| PRM-8KE-100G-RTU | Premium Right-to-Use 100G for Cisco 8000E Series              |
| 8KSW-B-SIA-3     | 8000 Type B Device SIA for 3-year term FCM 2.0                |
| 8KSW-B-SIA-5     | 8000 Type B Device SIA for 5-year term FCM 2.0                |



## Platform Description
Cisco 8712-MOD-M is a Cisco Silicon One™ based 2RU fixed router belonging to Cisco 8700 family of products, a first in class device to play interlligent Edge roles across Service Provider, Enterprise WAN , Data Center Interconnect, DC gateway network applications.

![fron-view.png]({{site.baseurl}}/images/fron-view.png)
Figure 1. Front view of the Cisco 8712-MOD-M  
{: .text-center}  


![fron-rear-full-chassi-view.png]({{site.baseurl}}/images/fron-rear-full-chassi-view.png)
Figure 2. Fron & Rear Full system view of Cisco 8712-MOD-M  
{: .text-center} 


![fron-panel-mgmnt-ports.png]({{site.baseurl}}/images/fron-panel-mgmnt-ports.png)
Figure 3. Front panel ports of Cisco 8712-MOD-M  
{: .text-center} 



## Modular Port Adaptors (MPAs)
Cisco 8712-MOD-M system has 4 slots to provision MPAs. There are 4 MPA variants supported on this system and any combination of MPA varinats can be plugged into the MPA 4 slots. And these MPAs are Online Insertion & Removal capable and user has the flexibility to choose the MPA varinats and number of MPAs based on the capacity provisioning of the network.

![mpa-bay-view.png]({{site.baseurl}}/images/mpa-bay-view.png){: .full}
Figure 4. MPA bay view of Cisco 8712-MOD-M  
{: .text-center}


### 8K-MPA-4D




### 8K-MPA-16H





### 8K-MPA-16Z4D





### 8K-MPA-18Z1D


## System Architecture
Cisco 8712-MOD-M system is powered by Cisco Silicon One™ K100 Network Processing Unit(NPU), a first in class to deliver intelligent Edge services. Get a kick start with: [Cisco-8000-architecture](https://xrdocs.io/8000/blogs/Cisco-8000-QOS-architecture/)To understand the fundamentals of Cisco Silicon One™. 

Cisco Silicon One™ K100 NPU features:

- 6.4Tbps full duplex forwarding capacity @2.6Bpps
	- has 2 network slices
	- each slice has a forwarding capacity of 3.2Tbps
	- has 128x56G SerDes
	- 2 Interfaec Groups (IFGs) per slice
    - has 64MB on-chip packet memory
	- has 8GB HBM deep buffering memory co-packaged with NPU

- On-chip crypto engine for MACsec & IPsec applications
- Multiple embedded ARC processors for CPU offloading
- 128K VoQ (Virtual Output Queue)
- 1M counters to facilitate statistics punching for different feature applications and built-in counters in HCAM(Hash-based TCAM)
- 6M IPv4 or 6M IPv6 FIB qualified scale without compression








<b> 8712-MOD-M System Block Diagram </b>

![sys-blocks.png]({{site.baseurl}}/images/sys-blocks.png)
