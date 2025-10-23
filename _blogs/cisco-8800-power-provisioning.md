---
published: true
date: '2022-03-22 13:20 -0400'
title: Cisco 8800 Power Provisioning
author: Lane Wigley
position: top
tags:
  - iosxr
  - Cisco 8000
  - Power
excerpt: >-
  Cisco’s 8000 Series’ introduced a new model for power allocation that reduces
  the amount of power that must be connected. This saves money and frees up
  facility resources for other uses.
---
In an earlier [post]({{site.url}}/8000/blogs/8000-router-power-consumption-part-1/), I discussed the importance of system power when assessing environmental impact and power costs. This article will explore another critical aspect – power provisioning. 

Provisioned power can be viewed as the wattage the operating system requests for the installed hardware (software provisioning) or the number and capacity of power supplies that must be installed based on the OS requirements (facility provisioning).

The goal of power provisioning software is to ensure that the system never runs out. This class of failure is called a brown out and is quite messy when it happens. When a router tries to pull more power than the PSUs can provide, some components will fall outside their electrial requirements and stop working properly.

Spikes in power draw can happen quickly. If another router fails, load can increase and add to line card and fabric power. If facility cooling degrades, fan power may increase.  

Preventing brown outs is considered sacred. They are extremely rare because layer upon layer of protection against them is provided. Unfortunately, this requires connecting far more power than real-world systems are likely to consume.  

Normal operating conditions for most routers include temperatures below 30C, both power feeds operational, average port bandwidth below 30%, and packet sizes above 800B. This results in a large gap between typical and provisioned power. As systems like the Cisco 8000 Series make more and more optimizations to reduce power in normal operation, the difference becomes even greater. In the next section, we’ll discuss some of the things we’re doing to reduce the gap.  

## 8808 Example
Let’s look at an example of typical vs. max power in the 8808 system. In the graphic below, the power required for the RPs is shown in purple, line cards in green, and optics power in orange. Each full block represents approximately 100W.  

![blog-1b.GIF]({{site.baseurl}}/images/blog-1b.GIF)

RP power is relatively consistent. For the line cards, light green boxes show typical power and darker green boxes represent the additional power for max performance. The optics power is not related to ambient temperature or traffic load, but different types of modules have a range of power draws from no power for copper cables up to over 20W for 400G ZR+ modules.
Three different line card configurations are shown. From the bottom: 48-port 100 GbE with 2km optics, 36-port 400 GbE with ZR+ optics, and 36-port 400GbE with 2km optics. The range from the highest to lowest optics power is dramatic.  

![blog-2b-small.GIF]({{site.baseurl}}/images/blog-2b-small.GIF)

Similarly, the power required for the switch fabric is shown below where the lighter blocks are for a typical load. The fans have the greatest range of power. The small blue boxes on the left represent the draw for each fan tray under normal conditions. As cooling requirements increase, more fan power is needed. This extra power is shown in the boxes to the right.  

If we remove the chassis photo, the relative power for all major system components can be easily compared. The difference between typical and potential max power is highlighted.  

![blog-3-small.GIF]({{site.baseurl}}/images/blog-3-small.GIF)


## Motivation for a New Approach

Our goal is to reduce the power allocation without creating a risk of brown out.
Let’s use an analogy to illustrate what we’re trying to do. There is some “fat” that can be cut when we are certain that it will not be needed. An example of this this fat is allocation of power needed for a ZR+ optic when the port is empty or using a copper cable. There is also “muscle” that we might not be using at any given time, but it could be activated with little notice. Examples of muscle are the full fan power and the extra power required if a line card is hit with 100% bandwidth of the smallest packets it can support at line rate. The goal is to trim as much fat as possible without cutting into the muscle.  

## Cisco 8000 Series Provisioning Innovation
Recent 8000 Series innovation improves the estimates of worst-case power. This approach represents the exact hardware configuration of the chassis. This preserves brownout protection while reducing the required power requested relative to the older design.  

The 8000 Series was developed by the team that built the ASR 9000, CRS, NCS 5500 and other systems that run IOS XR. The traditional approach to line card power provisioning is to bundle optics with the rest of the card. In the first IOS XR systems like the CRS-1, the Packet Over SONET interfaces (622 Mbps OC-3 to 40 Gbps OC-768) were part of the card, so modular optics weren’t even a consideration. When modular optics were introduced, their power was relatively consistent. In the SP space, a single form factor (10km LR4) was dominant, and most optics reaches consumed similar power.  

With 400 GbE, the optical modules require much more power and have a wide range among different reaches. Copper cables (requiring no power) are common in data centers, especially with the 8100 and 8200 Series. Grey optics (100m to 10km) require about 12W, ZR and ZR+ modules require 20+W. With the traditional approach, each 8800 Series 36x 400 GbE line card would have to allocate almost 800W to provision for 36 coherent optics modules.  

The 8000 Series addresses this challenge by provisioning optics power as modules are inserted. This change allows a system with copper cables and grey optics to use fewer power supplies than a system in a [Routed Optical Networking](https://www.cisco.com/c/en/us/solutions/service-provider/routed-optical-networking/index.html) design with many coherent ZR and ZR+ modules.  

The 8000 Series takes a similar approach to optimize provisioning in other areas. If an 8800 system is configured with fewer than the maximum 8 fabric cards, some internal links are powered down and the provisioned (and actual) line card power is reduced.  

The 8800 Series provisioning takes the following factors into account.  

-	Max fan power
-	Max route processor power
-	Max switch fabric power
    - Adjusted for number and speed of line cards present
-	Max line card power – unique for each card type
    -	Adjusted for number of fabric cards present
    - Adjusted for optics with different values for each module type
    - Adjusted for ports with MACsec enabled or disabled
    - Adjusted for power/performance mode (LC0/Q200 generation and later)

In addition to power for hardware present in the system, a small amount of power is allocated for hardware not yet installed.  

## Example of New vs Old Allocation Models
The example below represents a common customer configuration. This router has six 36x 400G line cards. The customer is transitioning from 100G to 400G, so each card has 18 QSFP28 100G LR4 and 18 QSFP56-DD 400G FR4 modules. These optics consume 4W and 12W respectively, for a total of 1728W. This is reflected in the new provisioning. The old approach required allocating 216 ports of max-power optics for a total of 5184W – almost 3500W of fat!  

Due to their reduced bandwidth requirements with the 100G ports and to save power, they are using 5 fabric cards.  In addition to reduced fabric power from the 3 empty FC slots, this saves some line card power for the links that would have connected to them.  

In the table below, the actual typical power is shown along with the old and new power allocation schemes. The largest difference is from the optics, but there are savings in line and fabric card allocation as well.  

|                      | #   | Actual / PID | Actual Subtotal | Old   | New   |
| -------------------- | --- | ------------ | --------------- | ----- | ----- |
| RP                   | 2   | 80           | 160             | 190   | 190   |
| Line Cards (36x 400) | 6   | 650          | 3900            | 5988  | 5604  |
| Empty LC             | 2   | 0            | 0               | 204   | 204   |
| Optics               | 216 |              | 1728            | 5184  | 1628  |
| Fabric (5 FCs)       | 5   | 370          | 1850            | 2195  | 1975  |
| Fans                 | 4   | 32           | 128             | 2748  | 2748  |
| Total                |     |              | 7766            | 16509 | 12349 |
| \# 48V DC 60A PSUs   |     |              |                 | 4     | 3     |
| w/ feed redundancy   |     |              |                 | 8     | 6     |

The bottom line is that this customer can reach A+B feed redundancy with six 4400W PSUs rather than eight in the old model. This is a yearly savings of approximately $20K per router with common US power and cooling costs for network facilities. Beyond reducing the router’s power cost, this frees up power and cooling capacity for servers or other network equipment.  

## Summary
In addition to the Cisco’s 8000 Series’ hardware innovations to optimize power, we have developed a new model for power allocation that reduces the amount of power that must be connected. This saves money and frees up facility resources for other uses.
