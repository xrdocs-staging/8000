---
published: true
date: '2022-03-07 15:40 +0100'
title: 'High Bandwidth Router Power Consumption (part 1) '
author: Lane Wigley
excerpt: >-
  Power claims for networking equipment are often difficult to understand. It’s
  not always clear whether chip, card, or system power is being stated and what
  conditions were used for the evaluation. Let’s take a step back and consider a
  better way of keeping score. 
tags:
  - iosxr
  - Cisco 8000
  - Power
position: top
---
{% include toc icon="table" title="Router Power Consumption (part 1)"%}

Power claims for networking equipment are often difficult to understand. It’s not always clear whether chip, card, or system power is being stated and what conditions were used for the evaluation. If we’re going to pursue more energy-efficient routers and switches, we need to understand what that really means.    

Let’s take a step back and consider a better way of keeping score. In my opinion – hopefully soon yours too – these are the day-to-day type system power consumption and the requirements for provisioned power (number and capacity of PSUs to support redundancy and worst-case conditions). This post will focus on exploring the components of system level power consumption. [Part 2]({{site.url}}/8000/blogs/cisco-8800-power-provisioning/) of this series focuses on power provisioning. 

Let’s start by defining a unit for the metric. I propose “watts per 100G without optics” based on system power consumption. It represents a common unit of bandwidth and (for high end routers) currently yields numbers in the 1-50W range that most of us are comfortable with. Optics should be subtracted because they are highly variable based on module type (e.g., 400G ZR+ is twice the power of FR4). As optics may impact cooling requirements, a more specific definition of “Watts per 100G at typical temperature with 2km optics minus optics module power” is a good benchmark. Variations on this, such as ZR+ optics or fewer cards or ports, may also be defined for operator-specific evaluations.

## System Power
System power consumption is the key metric to focus on. Chip power is an interesting way to compare devices, but it’s not relevant when it comes to paying the power bill or assessing environmental impact. Let’s look at what makes up system power by reviewing the following key components:

- Electrical data path
- Optics
- Cooling
- Mechanical design
- Control plane
- Losses in voltage conversion and signaling

## Electrical Data Path 

The key components of the electrical data path power comprise PHYs, retimers, and forwarding ASICs. PHYs are small devices such as gearboxes which combine SerDes (Serializer/Deserializers are used for fast I/O through PCBs) or split them into different speeds. A common gearbox operation is to convert 2x 56G SerDes from the ASIC to 4x 28G SerDes connected to a QSFP28 100 GbE port. The 100G ports on the 8201 and 8202 use this design. Retimers are used to extend electrical signals. Retimers are commonly used for long traces such as connecting optics to ASICs in the “back row” of a line card.  

Power-wise, ASICs are the stars of the show, so it’s understandable that they get most of the attention. They are the most complex components and are the key driver of system power for large, modular systems like the 8800. ASIC power can be broken into three main categories: SerDes, idle core power, and dynamic core power. In high-bandwidth chips like the Cisco Silicon One Q200, these are roughly equal. I’ll be focusing on this class of chips in this paper. It includes devices such as Broadcom’s Jericho 2 (used in NCS 5500) and Trident/Tomahawk (NCS 5000). Edge-class chips such as Cisco’s Lightspeed Plus and have similar characteristics but consume more power in complex per-packet operations (e.g., more counters) and less in SerDes due to lower bandwidth.  

Each of these categories has different characteristics of power consumption. SerDes require constant power independent of traffic load, but they can be powered down. On the 8000 Series, this occurs when ports are shut down or when a fabric or line card slot is empty. In the latter case, the corresponding SerDes on the connecting card are shut down (e.g., some Fabric Card (FC) SerDes are disabled when Line Card (LC) slots are empty).  

ASIC idle power is exactly what it sounds like. It’s the power required to keep the chip’s internal blocks up and running with no traffic load. Dynamic power occurs when packets are forwarded. Some dynamic power is consumed for per-packet operations (pps load) and some is needed for moving the bandwidth along the forwarding path (bps load).  

Core (non-SerDes) ASIC power will vary greatly among devices as networking ASICs serve many roles with a variety of requirements. Core ASIC power may vary based on several factors:  
- Function – DC vs. SP Edge vs. SP Core perform different levels of per-packet operations  
- Network vs. fabric capacity – ASIC I/O that only can connect to a switch fabric are much simpler and may be lower power  
- Process node (7nm is the current generation, 16nm and 28nm were the previous TSMC nodes)  
- Design – Newer microarchitectures tend to have better efficiency than chips carrying legacy constraints. The skill of the design engineers is also significant and shows up in the power bottom line.  
- Device temperature – A hot ASIC has marginally higher power than a cooler one.  
- Load – The number of transistors flipping at a given time (mainly a function of pps and bandwidth, but also of the design’s ability to gate unused logic)  

Most legacy SP ASIC designs were optimized for large modular systems with switch fabrics. This approach resulted in some of the ASIC I/O (usually a bit more than 50% to account for overhead and provide speedup) being hard coded for fabric operations. This greatly simplifies the design relative to chips that can dedicate their full bandwidth to the network. The logic for fabric lookups is dramatically simpler than network operations and, thus, requires less silicon real estate.  

The downside of fabric-bound I/O shows up when used in single-chip systems. The fabric bandwidth is lost and cannot be redirected to the network. This can be offset by using two chips back-to-back with a point to point “fabric”, but that is still less efficient than a single chip that can connect all its bandwidth to the network. When two ASICs are required, the chip power is doubled. If more chips and a switch fabric are required for small (2-3 RU) systems, the power goes even higher.

## Optics 

Optics are the divas of router hardware. They collectively consume significant power, may be densely populated, provide a small footprint for heat sinks, and their max temperature is 50C cooler than ASICs. The thermal management of optical modules was one of the most difficult challenges for the initial mechanical design of 400G-class systems – and then ZR+ modules doubled the requirement from 11W (DR4/FR4) to 22W (ZR+).  

Optical module power varies by reach, but the power for each reach is relatively consistent among vendors. The system power to cool the modules varies. For example, it may be more difficult to cool a QSFP-DD module in a traditional chassis with vertical airflow (i.e., each module preheats air for the one above it) than a newer orthogonal chassis (direct cold air to all modules). The QSFP-DD and OSFP designs also impact power required for cooling. Both can support high power optics, but the QSFP-DD design allows for ongoing heat sink innovation customized to the chassis. This results in less airflow needed to cool the modules and thus lower system power than OSFP which locked in a standardized heat sink design that limited innovation.  
## Cooling 

Routers are currently air-cooled; fans push or pull air through the chassis. The airflow path is important as each component heats the air for downstream devices within the chassis. Fan power is highly variable among systems and different operating conditions. For many systems, the highest fan power may be ten times typical. This great range is due to the nature of fan power consumption. The power is a cubic function of the air moved. When fan speeds get into the 80-100% range, their power goes up dramatically. Therefore, software should only set the fans to maximum speed when absolutely necessary.

## Mechanical Design 

Cooling isn’t the only aspect of mechanical design. Other decisions include the card height, number and size of fans, board layouts, heat sinks, and more. For example, small differences in line card height can impact the size of heat sinks and how constricted the airflow is (possibly building up pressure). These both have an impact on chip temps and fan power.

## Control Plane 

The control plane includes CPUs, DRAMs, SSDs, internal ethernet switches, and small components such as consoles and management ethernet. CPU power is relatively low compared to ASICs. The other control plane components don’t consume significant power.

## Losses in Voltage Conversion and Signaling 

The visible components aren’t the only consumers of power. 5-10% of power is lost when the power supplies convert the facility voltage (e.g., 48V DC or 220V AC) to the system voltage (e.g., 12V or 54V DC). Further power is lost as the system voltage is converted to the levels used by the electronics (e.g., 3.3V for optics). Power is also lost to the resistance of the PCBs as signals move from device to device.  

In the past, these factors could be ignored by users, but as power consumption becomes more critical, they shouldn’t be any longer. The 8000 Series includes many “behind the scenes” optimizations in these areas such as new power supplies and more efficient internal power distribution.  
## Metrics for the Cisco 8000 Series 

At the beginning of this paper, the metric of watts per 100G without optics was proposed. Let’s conclude by looking at those values for some specific systems.


| System              | Bandwidth  | Typical Power (W) | Watts / 100G |
| ------------------- | ---------- | ----------------- | ------------ |
| 8201-32FH           | 12.8 Tbps  | 265               | 2.1          |
| 8804 (4x 14.4T LC)  | 43.2 Tbps  | 4200              | 7.2          |
| 8808 (8x 14.4T LC)  | 86.4 Tbps  | 8100              | 7.0          |
| 8812 (12x 14.4 LC)  | 172.8 Tbps | 13900             | 8.1          |
| 8818 (18x 14.4T LC) | 259.2 Tbps | 18900             | 7.6          |  

## Conclusion 

Much is made of chip-level power efficiency, but that is not the ultimate goal – merely a means to an end. Operators shouldn’t care if power is used to flip transistors or spin fans, they just want the system to be efficient. Evaluating systems should include benchmarks that can be standardized as well as network-specific configurations which may include multiple generations of hardware, a partially filled chassis, or different traffic loads.    

In summary, the key metric for power consumption is system power in your typical conditions. When router shopping, ask vendors for system-level data that meets your real requirements and for details on how they were tested or calculated. Make sure to also look closely at timelines for qualification and ramp to deployment so that you evaluate systems available in the same generation and do not compare deployable hardware with press releases.
