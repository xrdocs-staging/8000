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

## Introduction
The Cisco 8000 Series combines the revolutionary Cisco Silicon One™, IOS XR® software, and a set of clean sheet chassis to deliver a breakthrough in high-performance routers. 
High-performance networking systems have historically been divided into routing or switching classes, with distinct hardware and software. But, as the necessity for reliable networks proliferates, it is imperative that traditional networks shift to a new architecture to address exponential bandwidth needs, ubiquitous connectivity, security, better quality of service, low latency coupled with high reliability, availability, and serviceability of the infrastructure. The Cisco® 8000 Series portfolio completes this journey with the Cisco 8700 products.
The Cisco 8700 series is an addition to [the Cisco 8000 Series Routers](https://www.cisco.com/site/us/en/products/networking/routers/8000-series/index.html) powered by [Cisco Silicon One](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html)™  ASICs.  8700 products embody this cutting-edge innovation of the 8000 portfolio, offering advanced features engineered for seamless integration and scalability. Whether enhancing existing infrastructure or enabling new capabilities, the 8700 series products empower organizations to achieve their goals with unmatched efficiency and effectiveness. 

## Cisco 8711-32FH-M Overview  
Cisco 8711-32FH-M is a 1RU 12.8T fixed system based on a single P100 ASIC. With 16 ports of QSFP-DD800 and 16 ports of QSFP56-DD on the faceplate, Cisco 8711-32FH-M uses only 4 of the 6 slices of the P100, optimizing for higher scale and a broader L2 feature set than is currently capable on Q200 based fixed systems. 


## Video  
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
| Fans and PSU         | 6x Fan Trays 5+1 Redundancy<br> 2x PSUs 1+1 Redundancy<br> Port side intake airflow, Port side Exhaust airflow<br> 2kW AC & 2kW DC PSUs PSU and Fan Trays<br> Field Replaceable Unit                                                  |
| Software             | Supports Cisco IOS XR Software Release 24.3.1 and later<br> Enhanced Automation for onboarding, service provisioning and monitoring.<br> Programmable infrastructure with segment routing (SR, SRv6) and Multi-homing EVPN.  |
| Typical Power        | 740W w/o Optics                                                                                                                                                                                                       |
| Dimension            | 1RU, (H) 1.73 x (W) 17.3 x (D) 23.6 in. (4.40 x 43.9 x 60 cm)<br> 35.4 lbs (16.07 kg)                                                                                                                                     |
| Use Case             | Core, Aggregation, Peering, DC Core                                                                                                                                                                                   |  

Table 1. Cisco 8711-32FH-M key components  
{: .text-center}  



## Cisco 8711-32FH-M Use Cases  
### Aggregation, Core, Peering and DC Core with RON(Routed Optical Networking)       
![use-case.png]({{site.baseurl}}/images/use-case.png)  

As the user applications are becoming more dynamic, requiring differentiated user experiences network resources needs to be optimized in terms of power and real estate. Cisco 8711-32FH-M has been designed to cater the market requirement for total system bandwidth, port diversity, and MACsec encryption. The requirements far exceeded what could be accomplished with a single P100 NPU. Cisco 8711-32FH-M supports 12.8 Tbps of forwarding capacity. Supported various configurations of “400G only”, “800G Only” or “800G plus 400G combo” in 1RU height and 600mm depth form factor. It provides 32 ports of high speed 400G or 128 ports of 100G or 128 ports of low speed 10G/25G in the system.  
Cisco’s [Converged SDN Transport Solution](https://blogs.cisco.com/sp/routed-optical-networking-its-about-the-architecture), is an architecture that delivers efficient network utilization, Simplified single layer, reduced network complexity, faster time to market, automation empowerment, and differentiated service offering. The solution works by merging IP and Optical onto a single layer where all the switching is done at Layer 3. Routers are connected with standardized [400G ZR/ZR+/Bright(0dBm) ZR+ coherent pluggable optics](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/710x/b-interfaces-config-guide-cisco8k-r710x/m-zr-zrp-cisco-8000.html). With a single service layer based upon IP, flexible management tools can leverage [telemetry](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/telemetry/710x/configuration/guide/b-telemetry-cg-8000-710x.html) and [model-driven programmability](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/programmability/710x/b-programmability-cg-8000-710x.html) to streamline lifecycle operations.  

### Secure High-speed Data Center/Cloud interconnect and large Regional Hub/Branch router    
![Screenshot 2024-12-24 at 5.08.50 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-24 at 5.08.50 PM.png)  

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

## Platform Description  
Cisco 8711-32FH-M is a 1RU fixed router belonging to Cisco 8700 family of products, a variant of the Cisco 8000 business, intended to be the primary routing solution for next generation Core, Aggregation, Peering, and DC Core networks.  

![Screenshot 2024-12-21 at 10.59.30 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-21 at 10.59.30 PM.png)  

Figure 3. Front view of Cisco 8711-32FH-M Router  
{: .text-center}  

As shown in Figure 3, The front of the chassis has 16 QSFP-DD800 + 16 QSFD56-DD ports along with the IO ports used for management, console, USB, and timing synchronization. Each QSFPDD port has its own bi-color LED that can be used to display the link status indication. 

**Notes** : QSFP-DD800 is 800G capable version of the QSFP56-DD.  It is backward compatible to legacy QSFP56- DD and all QSFP modules.
{: .notice}  

![Screenshot 2024-12-22 at 11.31.06 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 11.31.06 AM.png)  

Figure 4. Mgmt/Timing Interface details  
{: .text-center}  

Front panel ports include management and Timing interfaces:
- Management interfaces  
  - 1x 10/100/1000 Mbps Management Ethernet port  
  - 1x USB 3.0 (Type A)  
  - RJ45 console port directly to CPU  
  - 1x 10 GbE Control plane Expansion Ethernet port (Not used)  
- Timing interfaces  
8711-32FH-M is capable of frequency, time, and phase synchronization. These can be sourced from dedicated interfaces GPS port or from timestamped 1588 packets received on a normal data port in the system. The time, phase and frequency are distributed through the system with a frequency and PPS signal to devices near the physical port where timestamps are applied on ingress and egress packets.  
In 8711-32FH-M, network synchronization clock can be sourced from any of the QSFPDD ports and from GPS (ToD, 10MHz, 1PPS ports).  
- 1x GPS interface  
  - ToD (Time of Day) with RJ45 port  
  - 1 PPS coax port  
  - 10 MHz coax port  
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

### Fan Tray   

Cisco 8711-32FH-M supports six Fan trays for system cooling. It supports N+1 redundancy. All the Fan Trays are hot swappable and get inserted from the rear side of the router. There are two types of fans available.  

- Port Side Intake or Front to back Airflow (Burgundy color)  
- Port Side Exhaust or back to front Airflow (Royal Blue)  

All Fan Trays must be the same color (direction) to work correctly.  

![Screenshot 2024-12-22 at 10.09.56 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 10.09.56 AM.png)  

Figure 7. Back view of 6x FAN-1RU-PI-V2 + 2x PSU2KW-ACPI (Burgundy Latches)  
{: .text-center}  

![Screenshot 2024-12-22 at 10.26.56 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 10.26.56 AM.png)  

Figure 8. PSI (Port-side Intake) Airflow of Cisco 8711-32FH-M Router  
{: .text-center}  


![Screenshot 2024-12-22 at 10.11.38 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 10.11.38 AM.png)  

Figure 9. Back view of 6x FAN-1RU-PE-V2 + 2x PSU2KW-ACPE (Blue Latches)  
{: .text-center}  

![Screenshot 2024-12-22 at 10.37.44 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 10.37.44 AM.png)  

Figure 10. PSE (Port-side Exhaust) Airflow of Cisco 8711-32FH-M Router  
{: .text-center}  

Fan Tray has tricolor LED (one for each Fan Tray) of color Red, Yellow, and Blue  

![Screenshot 2024-12-22 at 10.16.36 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 10.16.36 AM.png)  

Figure 11. Fan Tray LEDs of Cisco 8711-32FH-M Router  
{: .text-center}  


### Power Supply Unit(PSU)  

871-32FH-M has two PSUs supporting 1+1 redundancy. Redundancy will be supported for High Line Voltage mode(200Vac to 240Vac) of AC supply. When AC supply is used in low line volage(100Vac to 127Vac), redundancy is not supported.  

**Note**: The Cisco 8711-32FH-M router doesn’t support a mix of AC and DC Power Supply Units (PSUs)  
{: .notice}  

AC Power Supply Unit (PSU2KW-ACPI, PSU2K-ACPE)  
- The AC power modules are single feed with 2000W capacity at 220V. They also support operation at 110V with half the capacity    
- AC power modules are rated at 2000W maximum at 220V AC high line input, and 1000W maximum at 110V AC low line input  
- AC nominal range: 100 to 127V AC and 200 to 240V AC  
- AC full range: 90 to 140V AC and 180 to 264V AC; with extended range to 300V AC  

DC Power Supply Unit (PSU2KW-DCPI, PSU2K-DCPE)  
- The DC power modules are single feed with 2000W capacity at any specified input voltage  
- DC nominal range: -40 to -72V DC  

| PSU PID     | Input Voltage                     | Input Current(Max) | Output Power  | Output           |
|-------------|-----------------------------------|--------------------|---------------|------------------|
| PSU2KW-ACPI | 100Vac to 127Vac<br> 200Vac to 240Vac | 12A<br> 12A            | 1,000W<br> 2,000W | 12V/84A<br> 12V/167A |
| PSU2KW-ACPE | 100Vac to 127Vac<br> 200Vac to 240Vac | 12A<br> 12A            | 1,000W<br> 2,000W | 12V/84A<br> 12V/167A |
| PSU2KW-DCPI | -40Vd to -72Vdc                   | 60A                | 2,000W        | 12V/167A         |
| PSU2KW-DCPE | -40Vd to -72Vdc                   | 60A                | 2,000W        | 12V/167A         |  

Table 3. PSUs types of Cisco 8711-32FH-M  
{: .text-center}   

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show platform</span>     
Node              Type                     State                    Config state
--------------------------------------------------------------------------------
0/RP0/CPU0        8711-32FH-M(Active)      IOS XR RUN               NSHUT  
0/FB0             8711-32FH-M[FB]          OPERATIONAL              NSHUT  
0/FT0             FAN-1RU-PI-V2            OPERATIONAL              NSHUT  
0/FT1             FAN-1RU-PI-V2            OPERATIONAL              NSHUT  
0/FT2             FAN-1RU-PI-V2            OPERATIONAL              NSHUT  
0/FT3             FAN-1RU-PI-V2            OPERATIONAL              NSHUT  
0/FT4             FAN-1RU-PI-V2            OPERATIONAL              NSHUT  
0/FT5             FAN-1RU-PI-V2            OPERATIONAL              NSHUT  
0/PM0             <mark>PSU2KW-ACPI</mark>              OPERATIONAL              NSHUT    
0/PM1             <mark>PSU2KW-ACPI</mark>              OPERATIONAL              NSHUT     

</code>
</pre>
</div>   

On this router, there are two Power Supply Units in Cisco 8711-32FH-M  

<div class="highlighter-rouge">
<pre class="highlight">
<code>   

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show environment power</span>   
================================================================================
CHASSIS LEVEL POWER INFO: 0
================================================================================
   Total output power capacity (Group 0 + Group 1) :    2000W +     2000W
   Total output power required                     :    1187W
   Total power input                               :     913W
   Total power output                              :     789W
Power Group 0:
================================================================================
   Power       Supply         ------Input----   ------Output---     Status
   Module      Type            Volts     Amps    Volts     Amps    
================================================================================
   0/PM0       <mark>PSU2KW-ACPI</mark>     207.5     2.3     12.1      34.6     OK
Total of Group 0:              477W/2.3A         418W/34.6A
Power Group 1:
================================================================================
   Power       Supply         ------Input----   ------Output---     Status
   Module      Type            Volts     Amps    Volts     Amps    
================================================================================
   0/PM1       <mark>PSU2KW-ACPI</mark>     207.5     2.1     12.1      30.7     OK
          
Total of Group 1:              435W/2.1A         371W/30.7A
================================================================================
   Location     Card Type               Power       Power        Status
                                        Allocated   Used
                                        Watts       Watts
================================================================================
   0/RP0/CPU0   8711-32FH-M             917         -            ON
   0/FT0        FAN-1RU-PI-V2           45          9            ON
   0/FT1        FAN-1RU-PI-V2           45          9            ON
   0/FT2        FAN-1RU-PI-V2           45          10           ON
   0/FT3        FAN-1RU-PI-V2           45          9            ON
   0/FT4        FAN-1RU-PI-V2           45          9            ON
   0/FT5        FAN-1RU-PI-V2           45          9            ON

</code>
</pre>
</div>   

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show environment fan</span>  
=============================================================
                                              Fan speed (rpm)
Location      FRU Type                        FAN_0    FAN_1   
-------------------------------------------------------------

0/FT0        FAN-1RU-PI-V2                     19080   16650   
0/FT1        FAN-1RU-PI-V2                     19020   16770   
0/FT2        FAN-1RU-PI-V2                     19230   16860   
0/FT3        FAN-1RU-PI-V2                     19110   16740   
0/FT4        FAN-1RU-PI-V2                     19050   16680   
0/FT5        FAN-1RU-PI-V2                     19080   16650   
0/PM0        <mark>PSU2KW-ACPI</mark>                       14656   13312  
0/PM1        <mark>PSU2KW-ACPI</mark>                       14560   13408

</code>
</pre>
</div>  

### 8711-32FH-M CPU complex card   

8711-32FH-M uses common CPU card designed for Cisco 8000 fixed router.  

CPU complex card is designed with Intel 6 core processor. This processor running IOS XR operating system manages the system by setting up the control plane, NPU, and communicates with various subsystems.  

| Device         | Configuration         |
|----------------|-----------------------|
| CPU            | Intel 6 Core @ 2.5GHz |
| Memory         | 64 GB DDR4 SODIMM     |
| M.2 SATA Drive | 480 GB SATA SSD       |
| USB            | 1 x USB3.0            |
| MGMT           | MGMT copper           |
| Console        |                       |
| Aikido FPGA    | Aikido Secure Boot    |  

### IOS XR Software 

The Cisco 8711-32FH-M is launched with IOS XR 24.3.1  

<div class="highlighter-rouge">
<pre class="highlight">
<code>   

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show version</span>
Cisco IOS XR Software, <mark>Version 24.3.1 LNT</mark>
Copyright (c) 2013-2024 by Cisco Systems, Inc.
Build Information:
 Built By     : sajshah
 Built On     : Wed Sep 04 18:28:32 UTC 2024
 Build Host   : iox-ucs-002
 Workspace    : /auto/srcarchive11/prod/24.3.1/8000/ws
 Version      : 24.3.1
 Label        : 24.3.1-iso
cisco 8000 (Intel(R) Xeon(R) CPU D-1633N @ 2.50GHz)
cisco 8711-32FH-M (Intel(R) Xeon(R) CPU D-1633N @ 2.50GHz) processor with 64GB of memory
8711-32FH-M uptime is 25 minutes
<mark>Cisco 8711 1RU 12.8T P100 System</mark>

</code>
</pre>
</div>   

For a complete list of supported features, refer to the [Cisco Feature Navigator](https://tools.cisco.com/ITDIT/CFN/jsp/index.jsp). 
[Cisco Software Download](https://software.cisco.com/download/home/286325065/type/280805694/release/7.3.1) page to download the Cisco IOS XR software images.  
Detailed information on IOS XR can be found in the [IOS XR data sheet](https://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-xr-software/datasheet-c78-743014.html).  


## Cisco 8711-32FH-M Port Identification
<b>Port Numbering</b>

![Screenshot 2024-12-23 at 12.25.47 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-23 at 12.25.47 AM.png)  

Cisco 8711-32FH-M port numbering follows that of physical interfaces as {Type}{R/S/I/P} or {Type}{R/S/I/P/B}  

![Screenshot 2024-12-22 at 12.24.45 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 12.24.45 PM.png)  


Native Interface Use-case with 100 GbE at 400 G port  

<div class="highlighter-rouge">
<pre class="highlight">
<code>   

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show ip interface brief</span>
Interface                      IP-Address      Status          Protocol Vrf-Name
<mark>HundredGigE0/0/0/0</mark>             40.0.0.1        Up              Up       default 
<mark>HundredGigE0/0/0/0.1</mark>           11.0.1.1        Up              Up       default 
HundredGigE0/0/0/0.2           11.0.2.1        Up              Up       default 
HundredGigE0/0/0/0.3           11.0.3.1        Up              Up       default 
HundredGigE0/0/0/0.4           11.0.4.1        Up              Up       default 
HundredGigE0/0/0/0.5           11.0.5.1        Up              Up       default 
HundredGigE0/0/0/0.6           11.0.6.1        Up              Up       default 
HundredGigE0/0/0/0.7           11.0.7.1        Up              Up       default 
HundredGigE0/0/0/0.8           11.0.8.1        Up              Up       default 
HundredGigE0/0/0/0.9           11.0.9.1        Up              Up       default 
<mark>HundredGigE0/0/0/0.10</mark>          11.0.10.1       Up              Up       default

</code>
</pre>
</div>  

Breakout Interface Use-case with 2x400 GbE at 800G port ( Even port, Top row ports)  

<div class="highlighter-rouge">
<pre class="highlight">
<code>  
hw-module port-range 0 1 instance 0 location 0/RP0/CPU0 mode <span style="background-color: #00FF00">2x400</span>  

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show ip interface brief</span>
Interface                      IP-Address      Status          Protocol Vrf-Name
<mark>FourHundredGigE0/0/0/0/0</mark>      unassigned      Up              Up       default 
<mark>FourHundredGigE0/0/0/0/1</mark>      unassigned      Up              Up       default

</code>
</pre>
</div>  

Breakout Interface Use-case with 8x100 GbE at 800G port ( Even port, Top row ports)  

<div class="highlighter-rouge">
<pre class="highlight">
<code>    
hw-module port-range 2 3 instance 0 location 0/RP0/CPU0 mode <span style="background-color: #00FF00">8x100</span>     

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show ip interface brief</span>
Interface                      IP-Address      Status          Protocol Vrf-Name
<mark>HundredGigE0/0/0/2/0</mark>          unassigned      Down            Down     default 
HundredGigE0/0/0/2/1          unassigned      Down            Down     default 
HundredGigE0/0/0/2/2          unassigned      Down            Down     default 
HundredGigE0/0/0/2/3          unassigned      Down            Down     default 
HundredGigE0/0/0/2/4          unassigned      Down            Down     default 
HundredGigE0/0/0/2/5          unassigned      Down            Down     default 
HundredGigE0/0/0/2/6          unassigned      Down            Down     default 
<mark>HundredGigE0/0/0/2/7</mark>          unassigned      Down            Down     default  

</code>
</pre>
</div>     

## System Details   

### System Block Diagram   

The Cisco 8711-32FH-M has one common CPU complex board, P100 NPU, 2 Power Supply Units (PSU) with support of (1 + 1) redundancy and 6 Fan Tray (FT) with support of 1 Fan Failure condition. – These PSUs and FTs are field replaceable units (FRUs).  

![Screenshot 2024-12-22 at 1.08.04 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 1.08.04 PM.png)  

Figure 12. Cisco 8711-32FH-M Block Diagram  
{: .text-center}  

P100 is a family of high-capacity packet switching devices.  P100 consists of 6 slices. Each slice has a Receive and a Transmit part (Rx slice and Tx slice). Each slice supports two groups of interfaces (i.e., a pair of IFGs). Performance wise, each slice handles up to one packet per clock and has a data bandwidth of up to 3.2 Tbps. The total capacity of a P100 device is determined by the number of slices. With four slices used in Cisco 8711-32FH-M and up to 5.047 Bpps forwarding performance per system.  

Cisco Silicon One P100 features:  
- 19.2 Tbps full-duplex and 8.1 Bpps Forwarding Capacity NPU  
  - 192x 100G SerDes with each capable of operating at 10G/25G/50G/100G using NRZ or PAM4 modulation  
  - Flexible port configuration supporting 10/25/40/50/100/200/400/800 Gbps  
  - 72 MB fully shared on-die packet buffer  
- Expandable in-package packet buffer using 8 GB HBM  
- On-chip, high-performance, P4-programmable host NPU for high-bandwidth offline packet processing  
- Multiple embedded processors for CPU offloading  
- 64K VoQ (Virtual Output Queue) for Network slice and 16K VoQ for Fabric slice  
- Dedicated 132 Counters banks (1M counters) and built-in counters in HCAM  
- 6M IPv4 or 3M IPv6 FIB qualified scale without compression (Hardware capable much more)  

![Screenshot 2024-12-22 at 6.37.52 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 6.37.52 PM.png){: .align-center}          
Figure 13. Cisco Silicon One P100  
{: .text-center}  


![Screenshot 2024-12-22 at 6.38.03 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 6.38.03 PM.png){: .align-center}    
Figure 14. Cisco Silicon One P100 Details  
{: .text-center}  


Cisco 8711-32FH-M uses only 4 of the 6 slices of the P100, optimizing for higher scale and a broader L2 feature set than is currently capable on Q200 based fixed systems.  

![Screenshot 2024-12-22 at 6.38.13 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 6.38.13 PM.png)  
Figure 15. Cisco Silicon One P100 in 8711-32FH-M  
{: .text-center}   

For more understanding on Cisco Silicon One P100 refer to [Cisco Silicon One P100-Info](https://www.cisco.com/c/en/us/solutions/collateral/silicon-one/silicon-one-p100-processor-ds.html).    


<b>Port/IFG/Slice assignment to P100 based Cisco 8711-32FH-M Router</b>  

There is the output of Port-IFG(Interface Group)-Slice mapping information.  

![Screenshot 2024-12-22 at 6.08.51 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 6.08.51 PM.png){: .align-center}    
Figure 16. Port-IFG-Slice mapping of Cisco 8711-32FH-M  
{: .text-center}  

<div class="highlighter-rouge">
<pre class="highlight">
<code>    

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show controllers npu voq-usage interface all instance all loc 0/rp0/cpu0</span>
----------------------------------------------------------------------------------------
Node ID: 0/RP0/CPU0
Intf         Intf     NPU Slice IFG  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    #    #    #   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------
<span style="background-color: #00FF00">Hu0/0/0/0</span>    78000130   0   <span style="background-color: #00FF00">3    1</span>     4    576      0        local    100G      7
Hu0/0/0/1    78000158   0   3    1     9    584      0        local    100G      7
FH0/0/0/2    78000228   0   3    1    35    824      0        local    400G      7
FH0/0/0/3    78000220   0   3    1    34    816      0        local    400G      7
FH0/0/0/4    78000218   0   3    0    33    808      0        local    400G      6
FH0/0/0/5    78000210   0   3    0    32    800      0        local    400G      6
FH0/0/0/6    78000160   0   3    0    10    640      0        local    400G      6
<span style="background-color: #00FF00">FH0/0/0/7</span>    78000208   0   <span style="background-color: #00FF00">3    0</span>    31    648      0        local    400G      6
<span style="background-color: #FFA500">FH0/0/0/8</span>    78000168   0   <span style="background-color: #FFA500">2    1</span>    11    656      0        local    400G      5
FH0/0/0/9    78000170   0   2    1    12    664      0        local    400G      5
FH0/0/0/10   78000178   0   2    1    13    672      0        local    400G      5
FH0/0/0/11   78000180   0   2    1    14    680      0        local    400G      5
Hu0/0/0/12/0 78000360   0   2    0    15    696      0        local    100G      4
Hu0/0/0/12/1 78000368   0   2    0    16    832      0        local    100G      4
Hu0/0/0/12/2 78000370   0   2    0    39    840      0        local    100G      4
Hu0/0/0/12/3 78000378   0   2    0    40    848      0        local    100G      4
Hu0/0/0/12/4 78000380   0   2    0    41    856      0        local    100G      4
Hu0/0/0/12/5 78000388   0   2    0    42    864      0        local    100G      4
Hu0/0/0/12/6 78000390   0   2    0    43    872      0        local    100G      4
Hu0/0/0/12/7 78000358   0   2    0    38    688      0        local    100G      4
FH0/0/0/14   78000198   0   2    0    17    704      0        local    400G      4
<span style="background-color: #FFA500">FH0/0/0/15</span>   780001a0   0   <span style="background-color: #FFA500">2    0</span>    18    712      0        local    400G      4
Hu0/0/0/16   78000150   0   1    1     8    632      0        local    100G      3
FH0/0/0/17   780001a8   0   1    1    19    720      0        local    400G      3
FH0/0/0/18   780001b0   0   1    1    20    728      0        local    400G      3
Hu0/0/0/19   78000148   0   1    1     7    624      0        local    100G      3
FH0/0/0/20/0 780001b8   0   1    0    21    616      0        local    400G      2
FH0/0/0/20/1 780001c0   0   1    0    22    736      0        local    400G      2
FH0/0/0/22   780001c8   0   1    0    23    744      0        local    400G      2
Hu0/0/0/23   78000140   0   1    0     6    608      0        local    100G      2
Hu0/0/0/24   78000138   0   0    1     5    592      0        local    100G      1
FH0/0/0/25   780001d0   0   0    1    24    600      0        local    400G      1
FH0/0/0/26   780001d8   0   0    1    25    752      0        local    400G      1
FH0/0/0/27   780001e0   0   0    1    26    760      0        local    400G      1
FH0/0/0/28   780001e8   0   0    0    27    768      0        local    400G      0
FH0/0/0/29   780001f0   0   0    0    28    776      0        local    400G      0
FH0/0/0/30   780001f8   0   0    0    29    784      0        local    400G      0
FH0/0/0/31   78000200   0   0    0    30    792      0        local    400G      0

</code>
</pre>
</div>   

From the above output, we can see Port 0 to port 7 is connected to slice 3 and Port 8 to port 15 is connected to slice 2.  

### PHY   

IFG of P100 connects to the PHYs through 128 SerDes  each operating at 100G data rate
The PHY can supports the followings:  

- Up to full 1.6 Tbps capacity per PHY    
- 16 SerDes in host (P100 NPU) side each SerDes operating at 100G  
- 16 SerDes in the line (Front Panel Ports)  side supporting at 100G (Retimer/800G  mode)  
- 32 SerDes in the line (Front Panel Ports)  side support at 50G (RGB mode)  
- PHY controlled from FPGA  
- SynE clock recovery and PTP (Class C timing) supported by PHY  
- Line-rate MACsec(XR support) or IPsec (HW ready)  

![Screenshot 2024-12-22 at 7.48.32 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-22 at 7.48.32 PM.png)  
Figure 17. NPU to PHY to QSFPDD Serdes Connectivity  
{: .text-center}   

### Front Panel Port combo configurations  

Cisco 8711-32FH-M can support several interface combinations.  

The first of the configurations is 32x Grey 400 GbE QSFP56-DD, in this configuration the QSFPDD ports are populated with 400G Grey optics. This is the default mode(without manual configuration). There is no restriction.  

![Screenshot 2024-12-23 at 8.05.33 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.05.33 PM.png)  

The second of the configurations is 32x Grey 100 GbE QSFP, in this configuration the QSFPDD ports are populated with 100G optics not 400G, the QSFP28 ports are populated with 100G optics without manual configuration. There is no restriction.

![Screenshot 2024-12-23 at 8.07.53 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.07.53 PM.jpg)  

The third of the configurations is 400G DCO (Digital Coherent Optic) optics, in this configuration 400G DCO ports are populated with 400G ZR/ZR+/Bright ZR+ into even ports (0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30 : Top). 

![Screenshot 2024-12-23 at 8.10.15 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.10.15 PM.jpg)  

The fourth of the configurations is 400G DCO (Digital Coherent Optic) and Grey 400G optics combinations, in this configuration 400G DCO ports are populated with 400G ZR/ZR+/Bright ZR+ into even ports (0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30 : Top) and 400G Grey optic into Odd ports (1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31: Bottom).  

![Screenshot 2024-12-23 at 8.12.19 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.12.19 PM.jpg)  

The 5th of the configurations is 16x Grey 800G Grey Optics (QDD 2X400G FR4 or QDD 8X100G FR), in this configuration 800G ports are populated with 800G optics into even ports (0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30 : Top) and all other Odd ports(Bottom) are shutdown automatically.  

![Screenshot 2024-12-23 at 8.14.00 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.14.00 PM.jpg)  

The 6th of the configuration is Grey 800G Optics and 400G DCO (ZR/ZR+/Bright ZR+) Optics, in this configuration the number of 800G Grey and 400G DCO (ZR, ZR+, Bright ZR+) must sum to 16 on Even ports (0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30: Top). 

![Screenshot 2024-12-23 at 8.14.54 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.14.54 PM.jpg)  

The 7th of the configuration is Grey 800G Optics, 400G DCO Optics (ZR, ZR+, Bright ZR+), and Grey 400G Optics combination, in this configuration the number of 800G Grey and 400G DCO  must sum to 16 on Even ports (0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30: Top). For every even port with 400G DCO (ZR, ZR+, Bright ZR+), the corresponding odd port can also have Grey 400G optics.  

![Screenshot 2024-12-23 at 8.15.53 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 8.15.53 PM.jpg)  

To verify optics supported, please use the Cisco Optics-to-Device Compatibility Matrix tool:
[the TMG matrix](https://tmgmatrix.cisco.com/).     
It contains details on the connector types, the reach, the minimum release required, etc.  


### Maximum port scale and support on Cisco 8711-32FH-M  

The following table represents the maximum number of ports the Cisco 8711-32FH-M router can support.  

|          | 10 GbE  | 25 GbE  | 40 GbE | 100 GbE | 400 GbE | 800G*        |
|----------|---------|---------|--------|---------|---------|--------------|
| Native   | Roadmap | Roadmap | 32     | 32      | 32      | N/A          |
| Breakout | 128     | 128     | N/A    | 128**   | 32**    | 16 Even ports |  


* 2x400G & 8x100G only
** 2x400G only
*** 4x100G, 8x100G 


- 10 GbE  
  - As Native 10G Optic with CVR-QSFP-SFP10G (QSA), fully populated into all 32 Ports in roadmap  
  - As Breakout, fully populated into all 32 Ports. 32 ports x (4x10 GbE) = 128 10 GbE total  
  
- 25 GbE
  - As Native 25G Optic , fully populated into all 32 Ports in roadmap  
  - As Breakout, fully populated into all 32 Ports. 32 ports x (4x25 GbE) = 128 25 GbE total  
  
- 40 GbE  
  - As Native 40G Optic , fully populated into all 32 Ports  
  
- 100 GbE  
  - As Native 100G Optic, fully populated into all 32 Ports   
  - As Breakout (Use case-2: 2x100 GbE), fully populated into all 32 Ports. 32 ports x (2x100 GbE) = 64 100 GbE total or   
  - As Breakout (Use case-2: 4x100 GbE), fully populated into all 32 Ports. 32 ports x (4x100 GbE) = 128 100 GbE total or   
  - As Breakout (Use case-3: 8x100 GbE), populated into 16 even ports(Top) x (8x100 GbE) = 128 100 GbE total   
  
- 400 GbE  
  - As Native 400G Optic, fully populated into all 32 Ports   
  - As Breakout with 2x 400 GbE, Populated into even 16 Ports (Top row). 16 Even ports (Top) x (2x400 GbE) = 32 400 GbE total  
  
- 800 G  
  - As Native 800G Optic, Not supported  
  - As Breakout, Used with 2x400 GbE or 8x100 GbE optics at Even ports (Top row)  
  
 **Other things to know?** 
- We will support 10 GbE optics (SFP-10G-LR-S, SFP-10G-SR-S, SFP-10G-ER-S)  via QSFP to SFP Adaptor (QSA: CVR-QSFP-SFP10G). Not supported in XR 24.4.1, in the roadmap  
- 1G will not be supported 

### 800G Transceiver  

**2x400G (QDD-2X400G-FR4) Optical Transceiver**    

![Screenshot 2024-12-23 at 11.01.37 PM.png]({{site.baseurl}}/images/Screenshot 2024-12-23 at 11.01.37 PM.png)    
Figure 18. QDD-2X400G-FR4 Optical connector  
{: .text-center}   

- Fiber: SMF  
- Connector: Dual Duplex LC  
- 4 Wave (nm): 1270,1290,1310,1330  
- Use-Case: 2 ports 400 GbE  


**8x100G (QDD-8X100G-FR) Optical Transceiver**  

![Screenshot 2024-12-23 at 11.03.42 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 11.03.42 PM.jpg)   
Figure 19. QDD-8X100G-FR Optical connector  
{: .text-center}   


- Fiber: SMF  
- Connector: Dual MPO-12 (APC)  
- Wave (nm): 1310 - 1 wavelength per fiber  
- Use-Case: 8 ports 100 GbE with 2x MPO-4xLC SMF breakout cable or 2 ports 400 GbE with MPO-MPO SMF   

### QSFP-DD800 Optics Use Cases  
Sample 800G Breakout config on Cisco 8711-32FH-M  

![Screenshot 2024-12-23 at 11.09.40 PM.jpg]({{site.baseurl}}/images/Screenshot 2024-12-23 at 11.09.40 PM.jpg)  

**Use-Case 1. QDD-2X400G-FR4 at Port 0 and QDD-8X100G-FR at Port 2**  
![Screenshot 2024-12-24 at 9.49.59 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-24 at 9.49.59 AM.png)  

<div class="highlighter-rouge">
<pre class="highlight">
<code>  
hw-module port-range 0 1 instance 0 location 0/RP0/CPU0 mode <span style="background-color: #00FF00">2x400</span>
hw-module port-range 2 3 instance 0 location 0/RP0/CPU0 mode <span style="background-color: #00FF00">8x100</span>
</code>
</pre>
</div>   

<div class="highlighter-rouge">
<pre class="highlight">
<code>  
RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show interfaces brief | inc FH0/0/0/0</span> 
               Intf       Intf        LineP              Encap  MTU        BW
               Name       State       State               Type (byte)    (Kbps)
--------------------------------------------------------------------------------
       <mark>FH0/0/0/0/0</mark>        down        down               ARPA  1514  400000000
       FH0/0/0/0/1        down        down               ARPA  1514  400000000


RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">show interfaces brief | inc Hu0/0/0/2</span> 
               Intf       Intf        LineP              Encap  MTU        BW
               Name       State       State               Type (byte)    (Kbps)
--------------------------------------------------------------------------------
       <mark>Hu0/0/0/2/0</mark>        down        down               ARPA  1514  100000000
       Hu0/0/0/2/1        down        down               ARPA  1514  100000000
       Hu0/0/0/2/2        down        down               ARPA  1514  100000000
       Hu0/0/0/2/3        down        down               ARPA  1514  100000000
       Hu0/0/0/2/4        down        down               ARPA  1514  100000000
       Hu0/0/0/2/5        down        down               ARPA  1514  100000000
       Hu0/0/0/2/6        down        down               ARPA  1514  100000000
       Hu0/0/0/2/7        down        down               ARPA  1514  100000000
</code>
</pre>
</div>  

**Use-Case 2. QDD-2X400G-FR4 at Port 0 and Grey 400G optics at Port 2 and Port 3**  
![UC2.png]({{site.baseurl}}/images/UC2.png)  

<div class="highlighter-rouge">
<pre class="highlight">
<code> 
hw-module port-range 0 1 instance 0 location 0/RP0/CPU0 mode <span style="background-color: #00FF00">2x400</span>  
</code>
</pre>
</div>

<div class="highlighter-rouge">
<pre class="highlight">
<code> 
RP/0/RP0/CPU0:8711-32FH-M#sh interfaces brief | inc FH0/0/0/0 
               Intf       Intf        LineP              Encap  MTU        BW
               Name       State       State               Type (byte)    (Kbps)
--------------------------------------------------------------------------------
      <mark>FH0/0/0/0/0</mark>        down        down               ARPA  1514  400000000
      <mark>FH0/0/0/0/1</mark>        down        down               ARPA  1514  400000000

RP/0/RP0/CPU0:8711-32FH-M#sh interfaces brief
               Intf       Intf        LineP              Encap  MTU        BW
               Name       State       State               Type (byte)    (Kbps)
--------------------------------------------------------------------------------
       <mark>FH0/0/0/2</mark>        down        down               ARPA  1514  400000000
       <mark>FH0/0/0/3</mark>        down        down               ARPA  1514  400000000
</code>
</pre>
</div>  

**Use-Case 3. QDD-8X100G-FR at Port 2 and Grey 400G Optics at Port 0 and Port 1**  
![UC3.png]({{site.baseurl}}/images/UC3.png)   

<div class="highlighter-rouge">
<pre class="highlight">
<code>   
hw-module port-range 2 3 instance 0 location 0/RP0/CPU0 mode <span style="background-color: #00FF00">8x100</span>  
</code>
</pre>
</div>  


<div class="highlighter-rouge">
<pre class="highlight">
<code>   
RP/0/RP0/CPU0:8711-32FH-M#show interfaces brief        
       Intf       Intf        LineP              Encap  MTU        BW
               Name       State       State               Type (byte)    (Kbps)
--------------------------------------------------------------------------------
       <mark>FH0/0/0/0</mark>        down        down               ARPA  1514  400000000
       FH0/0/0/1        down        down               ARPA  1514  400000000

RP/0/RP0/CPU0:8711-32FH-M#show interfaces brief | inc Hu0/0/0/2 
               Intf       Intf        LineP              Encap  MTU        BW
               Name       State       State               Type (byte)    (Kbps)
--------------------------------------------------------------------------------
       <mark>Hu0/0/0/2/0</mark>        down        down               ARPA  1514  100000000
       Hu0/0/0/2/1        down        down               ARPA  1514  100000000
       Hu0/0/0/2/2        down        down               ARPA  1514  100000000
       Hu0/0/0/2/3        down        down               ARPA  1514  100000000
       Hu0/0/0/2/4        down        down               ARPA  1514  100000000
       Hu0/0/0/2/5        down        down               ARPA  1514  100000000
       Hu0/0/0/2/6        down        down               ARPA  1514  100000000
       Hu0/0/0/2/7        down        down               ARPA  1514  100000000
</code>
</pre>
</div>  

For more understanding on Cisco QSFP-DD800 Transceiver Modules, refer to [Cisco QSFP-DD800 Transceiver Modules Data Sheet](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/transceiver-modules/qsfp-dd800-transceiver-modules-ds.html).   


### MACsec Support  
Starting IOS-XR 24.4.1, MACsec is supported on all ports of Cisco 8711-32FH-M. All ports are connected to PHY supporting the feature. It can be supported on 10G, 25G, 40G, 100G, 400G ports and also any breakout ports.  
![]({{site.baseurl}}images/Screenshot 2024-12-24 at 10.18.13 AM.png)  ![Screenshot 2024-12-24 at 10.18.13 AM.png]({{site.baseurl}}/images/Screenshot 2024-12-24 at 10.18.13 AM.png)  

- Full line-rate MACsec encryption engines to enable secure and low latency transport    
- Supports MACsec GCM-AES-128/256 and GCM-AES-XPN-128/256   
- Supports LAN MACsec and WAN MACsec  
- Supports “VLAN tag in the clear” in the roadmap  

**Notes**: k9 package is required for crypto data plane    
{: .notice}  

Cisco 8711-32FH-M can support the following MACsec Use Cases    
![MACsec-UC.png]({{site.baseurl}}/images/MACsec-UC.png)  

**Notes**: Use-case #3b and #4 in the roadmap  
{: .notice}  

| Use Cases   | Use Case Descriptions              | Cisco 8711-32FH-M |
|-------------|------------------------------------|-------------------|
| Use Case#1  | Link MACsec in MPLS/IP Topology    | Supported         |
| Use Case#2  | Link MACsec over LAG members       | Supported         |
| Use Case#3a | CE Port Mode MACsec over P2P L2VPN | Supported         |
| Use Case#3b | CE Port Mode MACsec over MP L2VPN  | Roadmap           |
| Use Case#4  | VLAN Clear Tags MACsec over L2VPN  | Roadmap           |    
  
Table 4. Cisco 8711-32FH-M MACsec Use Case supported     
{: .text-center}  

Cisco 8711-32FH-M can support the following features to interoperate over the service provider.   
- Ability to change MKA EAPoL Destination Address type  
- Ability to change MKA Ether-type value  
- Ability to configure Anti-replay window sizes   

[MACsec configurations on Cisco 8711-32FH-M](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/security/24xx/configuration/guide/b-system-security-cg-cisco8000-24xx/configuring-macsec.html)  


### Fan Tray Redundancy  
Cisco 8711-32FH-M provides 5+1 Fan Tray Redundancy. The system can operate in following conditions:  
•	6 Fan Trays is Ideal case  
•	5+1 Fan Tray, followed by major alarm. Speed up the all system Fans to 100%  


### Packet flow in Cisco 8711-32FH-M

![Packet flow.png]({{site.baseurl}}/images/Packet flow.png)  
Figure 20. Data path flow in Cisco 8711-32FH-M  
{: .text-center}   

1. Packets arrive on the input slice of the receive device    
2. The NPU performs the packet lookup, feature (example: ingress ACL) checks, and identifies the destination for the packet  
3. The ingress NPU enqueues the packet on the corresponding VoQ for the {destination, traffic class}  
4. When credits are available for the destination VoQ, the packet is switched across the SMS to the destination (egress) slice  
5. On the egress slice, the transmit direction lookups and feature (example: Egress ACL) checks are performed  
6. The final encapsulations are added to the packet and it’s transmitted from the TX NPU out the physical interface  
7. All packets are switched through the same SMS regardless of whether they are bound for the same NPU slice or a different NPU slice  

## Conclusion  

This document discussed the Cisco 8711-32FH-M architecture.  
The Cisco 8711-32FH-M is the P100 based fixed router. It offers a very high 10G, 25G, 100G, and 400G port density including MACsec and RON in a very small size (1RU).  
Cisco 8711-32FH-M is intended to be the primary routing solution for next generation Core, Aggregation, Peering, and DC Core networks.   

## Reference  

[Cisco Silicon One™](https://www.cisco.com/site/us/en/products/networking/silicon-one/index.html)    
[Cisco 8711-32FH-M Datasheet](https://www.cisco.com/c/en/us/products/se/2024/8/Collateral/8700-series-routers-ds.html)    
[Cisco 8711-32FH-M Hardware Installation Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/hardware/hig-fixed-8700/b-8700-hardware-installation-guide/routers_overview.html)    
[Cisco 8000 Configuration guide](https://www.cisco.com/c/en/us/support/routers/8000-series-routers/products-installation-and-configuration-guides-list.html)    
[Cisco Optics-to-Device Compatibility Matrix tool](https://tmgmatrix.cisco.com/)    
[Cisco 8000 Power Calculator](https://8000-power.cisco.com/)    

## Modification History  

| Version | Data     | Author(s)     | Comments            |
|---------|----------|---------------|---------------------|
| 1       | January 8, 2025 | Chang Soo Lee | Initial Publication |  

