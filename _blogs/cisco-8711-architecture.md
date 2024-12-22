---
published: true
date: '2024-08-23 18:44 +0200'
title: Cisco 8711-32FH-M Architecture White Paper
author: Chang Soo Lee
excerpt: >-
  This post will describe Cisco 8711-32FH-M architecture, based on Silicon One
  P100 ASIC.
tags:
  - Cisco 8000
  - Silicon One P100
  - Fixed System
position: hidden
---
{% include toc icon="table" title="Cisco 8711-32FH-M Architecture White Paper" %}

# Introduction
The Cisco 8000 Series combines the revolutionary Cisco Silicon One™, IOS XR® software, and a set of clean sheet chassis to deliver a breakthrough in high-performance routers. 
High-performance networking systems have historically been divided into routing or switching classes, with distinct hardware and software. But, as the necessity for reliable networks proliferates, it is imperative that traditional networks shift to a new architecture to address exponential bandwidth needs, ubiquitous connectivity, security, better quality of service, low latency coupled with high reliability, availability, and serviceability of the infrastructure. The Cisco® 8000 Series portfolio completes this journey with the Cisco 8700 products.
The Cisco 8700 series is an addition to [the Cisco 8000 Series Routers](https://www.cisco.com/site/us/en/products/networking/routers/8000-series/index.html) powered by [Cisco Silicon One](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html)™  ASICs.  8700 products embody this cutting-edge innovation of the 8000 portfolio, offering advanced features engineered for seamless integration and scalability. Whether enhancing existing infrastructure or enabling new capabilities, the 8700 series products empower organizations to achieve their goals with unmatched efficiency and effectiveness. 

# Cisco 8711-32FH-M Overview  
Cisco 8711-32FH-M is a 1RU 12.8T fixed system based on a single P100 ASIC. With 16 ports of QSFP-DD800 and 16 ports of QSFP56-DD on the faceplate, Cisco 8711-32FH-M uses only 4 of the 6 slices of the P100, optimizing for higher scale and a broader L2 feature set than is currently capable on Q200 based fixed systems. 

# Video  
<iframe width="560" height="315" src="https://www.youtube.com/embed/v13LpoQGXdI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>  



Cisco 8711-32FH-M addresses scaled requirements for spine/leaf and collapsed core, Aggregation, Peering, DC Core, 3rd party colocation, and RON use cases.  

![Figure1.png]({{site.baseurl}}/images/Figure1.png)
Figure 1. Front view of the Cisco 8711-32FH-M  
{: .text-center}  

![Figure2.png]({{site.baseurl}}/images/Figure2.png)
Figure 2. Rear view of the Cisco 8711-32FH-M  
{: .text-center}   

In the below table high level Cisco 8711-32FH-M overview is listed.   

| Items                | Descriptions                                                                                                                                                                                                          |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data Plane           | Single 12.8T Cisco Silicon One P100 (7nm)<br> 72 MB fully shared packet buffer & 8 GB HBM packet buffer                                                                                                                   |
| Control Plane        | Intel 6 Core x86 CPU<br> 64 GB DRAM / 480 GB SSD                                                                                                                                                                          |
| Port Configuration   | 16x QSFP-DD800 + 16x QSFP56-DD                                                                                                                                                                                        |
| Management Ports     | RS-232 console, 10 GbE Control Plane expansion, 10/100/1000Mbps Management,<br> 1x USB 3.0, GPS (ToD, 10MHz, 1PPS), GNSS                                                                                                  |
| Product Capabilities | 800G (QDD-2X400G-FR4, QDD-8X100G-FR)<br> Line-rate MACsec(XR support)/IPsec(HW ready) encryption at all ports<br> Class C timing<br> 400G RON ready (Supports DCO optics)                                                         |
| Fans and PSU         | 6x Fan Trays 5+1 Redundancy<br> 2x PSUs 1+1 Redundancy<br> Port side intake airflow, Port side Exhaust airflow<br> 2kW AC & 2kW DC PSUs PSU and Fan Trays Field Replaceable Unit                                                  |
| Software             | Supports Cisco IOS XR Software Release 24.3.1 and later.<br> Enhanced Automation for onboarding, service provisioning and monitoring.<br> Programmable infrastructure with segment routing (SR, SRv6) and Multi-homing EVPN.  |
| Typical Power        | 740W w/o Optics                                                                                                                                                                                                       |
| Dimension            | 1RU, (H) 1.73 x (W) 17.3 x (D) 23.6 in. (4.40 x 43.9 x 60 cm)<br> 35.4 lbs (16.07 kg)                                                                                                                                     |
| Use Case             | Core, Aggregation, Peering, DC Core                                                                                                                                                                                   |  

Table 1. Cisco 8711-32FH-M key components  
{: .text-center}  



# Cisco 8711-32FH-M Use Cases  
**Aggregation, Core, Peering and DC Core with RON(Routed Optical Networking)**     

![use-case.png]({{site.baseurl}}/images/use-case.png)  

As the user applications are becoming more dynamic, requiring differentiated user experiences network resources needs to be optimized in terms of power and real estate. Cisco 8711-32FH-M has been designed to cater the market requirement for total system bandwidth, port diversity, and MACsec encryption. The requirements far exceeded what could be accomplished with a single P100 NPU. Cisco 8711-32FH-M supports 12.8 Tbps of forwarding capacity. Supported various configurations of “400G only”, “800G Only” or “800G plus 400G combo” in 1RU height and 600mm depth form factor. It provides 32 ports of high speed 400G or 128 ports of 100G or 128 ports of low speed 10G/25G in the system.  
Cisco’s [Converged SDN Transport Solution](https://blogs.cisco.com/sp/routed-optical-networking-its-about-the-architecture), is an architecture that delivers efficient network utilization, Simplified single layer, reduced network complexity, faster time to market, automation empowerment, and differentiated service offering. The solution works by merging IP and Optical onto a single layer where all the switching is done at Layer 3. Routers are connected with standardized [400G ZR/ZR+/Bright(0dBm) ZR+ coherent pluggable optics](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/710x/b-interfaces-config-guide-cisco8k-r710x/m-zr-zrp-cisco-8000.html). With a single service layer based upon IP, flexible management tools can leverage [telemetry](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/telemetry/710x/configuration/guide/b-telemetry-cg-8000-710x.html) and [model-driven programmability](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/programmability/710x/b-programmability-cg-8000-710x.html) to streamline lifecycle operations.  

**Secure High-speed Data Center/Cloud interconnect and large Regional Hub/Branch router**  

![Screenshot 2024-12-21 at 10.44.17 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-21 at 10.44.17 PM.png)  

The requirements for securing the connection of multiple data center and cloud links typically target a smaller amount of links, while requiring the highest level of bandwidth requirements. Sometimes called data center interconnect (DCI), WAN MACsec is an ideal encryption solution for interconnecting data center or multilocation cloud environments.  

The backhauling of remote branch sites for government, enterprise, or commercial organizations is critical to any business. In the consumer space, this can be important for remote stores and point-of-sale kiosks, and in the enterprise and government space, it is crucial for remote agencies and offices.  

**Understanding the Cisco 8711-32FH-M Naming Logic**  

![Screenshot 2024-12-21 at 10.50.01 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-21 at 10.50.01 PM.png)   

Let us have a quick refresher of the Cisco 8711-32FH-M’s PIDs and description.  

| PIDs             | Product Description                                           |
|------------------|---------------------------------------------------------------|
| 8711-32FH-M      | Cisco 8711 1RU 12.8T P100 System                              |
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

Table 2. Cisco 8711-32FH-M PIDs  
{: .text-center}  

The RTUs (Right-to-Use licenses) provide customers with the ability to access and utilize specific perpetual software functionalities without the requirement to purchase the complete software package.  

The SIAs (Software Innovation Access licenses) are term-based agreements that provide customers with access to specific software benefits. The SIAs enable customers to optimize their software usage, easily manage licenses across their network infrastructure, and ensure seamless upgrades to the latest versions of IOS XR software.  

# Platform Description  
Cisco 8711-32FH-M is a 1RU fixed router belonging to Cisco 8700 family of products, a variant of the Cisco 8000 business, intended to be the primary routing solution for next generation Core, Aggregation, Peering, and DC Core networks.  

![Screenshot 2024-12-21 at 10.59.30 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-21 at 10.59.30 PM.png)  

Figure 3. Front view of Cisco 8711-32FH-M Router  
{: .text-center}  

As shown in Figure 3, The front of the chassis has 16 QSFP-DD800 + 16 QSFD56-DD ports along with the IO ports used for management, console, USB, and timing synchronization. Each QSFPDD port has its own bi-color LED that can be used to display the link status indication. 

**Notes** : QSFP-DD800 is 800G capable version of the QSFP56-DD.  It is backward compatible to legacy QSFP56- DD and all QSFP modules.
{: .notice}  

![Screenshot 2024-12-21 at 11.01.51 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-21 at 11.01.51 PM.png)  

Figure 4. Mgmt/Timing Interface details  
{: .text-center}  

Front panel ports include management and Timing interfaces:
- Management interfaces  
 -- 1x 10/100/1000 Mbps Management Ethernet port  
 -- 1x USB 3.0 (Type A)  
 -- RJ45 console port directly to CPU  
 -- 1x 10 GbE Control plane Expansion Ethernet port (Not used)  
- Timing interfaces  
8711-32FH-M is capable of frequency, time, and phase synchronization. These can be sourced from dedicated interfaces GPS port or from timestamped 1588 packets received on a normal data port in the system. The time, phase and frequency are distributed through the system with a frequency and PPS signal to devices near the physical port where timestamps are applied on ingress and egress packets.  
In 8711-32FH-M, network synchronization clock can be sourced from any of the QSFPDD ports and from GPS (ToD, 10MHz, 1PPS ports).  
- 1x GPS interface  
 -- ToD (Time of Day) with RJ45 port  
 -- 1 PPS coax port  
 -- 10 MHz coax port  
- 1x GNSS (Global Navigation Satellite System) receiver ( PRTC(Primary Reference Time Clock) Class B Accuracy) 

**Notes**: No BITS interface support on 8711-32FH-M  
{: .notice}   

As shown in Figure 5, There are also 4 LEDs on the front-panel that can be used to display the system status. The front panel faceplate is filled with perforations that provide airflow for cooling.  

![Screenshot 2024-12-22 at 9.57.25 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 9.57.25 AM.png)  

Figure 5. System status LEDs and descriptions details  
{: .text-center}  

![Screenshot 2024-12-22 at 9.57.38 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 9.57.38 AM.png)    

As shown in Figure 6, on the rear side of the chassis, there’re the followings:    
- 2 Power Supply Units (PSUs) with support of  (1 + 1) redundancy  
- Pull-out label: Serial number and PSU/Fan Tray location  
- 6 Fan Tray (FT) with support of 1 Fan Failure condition  
- PSUs and FTs are hot-swappable  

Figure 6. Back view of Cisco 8711-32FH-M Router with 6 FTs and 2 PSUs  
{: .text-center}  

This router supports both AC and DC Power Supply Unit providing the system power. The Fan Trays provide system cooling via dual airflow direction mechanism – front to back(PSI) and back to front(PSE).  


# Slot and Port identification

# System Details

# Redundancy

# Conclusion

# Reference

# Modification History
