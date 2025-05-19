---
published: true
date: '2025-05-16 11:27 +0200'
title: Cisco 8000 FIB Scale for Silicon One P100
author: Frederic Cuiller
tags:
  - Cisco 8000
  - Silicon One P100
  - FIB scale
position: top
excerpt: >-
  This article aims to document and demonstrate FIB scale and capabilities for
  Cisco 8000 routers running Silicon One P100 ASIC.
---
{% include toc icon="table" title="Cisco 8000 FIB Scale for Silicon One P100" %}

# Introduction
Similarly to the work done on Cisco 8000 running Silicon One Q100 and Q200 ASIC, this article aims to document and demonstrate FIB scale and capabilities for Cisco 8000 routers running Silicon One P100 ASIC.

This article is applicable to the following products:

- Cisco 8711-32FH-M: 12.8Tbps router, with MACsec
- Cisco 8212-48FH-M: 19.2Tbps router, with MACsec
- Cisco 88-LC1-36EH: 28.8Tbps LC, without MACsec
- Cisco 88-LC1-12TH24FH-E: 12Tbps LC, without MACsec
- Cisco 88-LC1-52Y8H-EM: 3.7Tbps LC, with MACsec

# Silicon One P100 FIB
The FIB structure and implementation is similar to Silicon One Q100 and Q200 ASIC. The main difference is the LPM scale.

![p100-fib.png]({{site.baseurl}}/images/p100-fib.png)
_Courtesy CS Lee, Cisco Live 8000 Techtorial_

The maximum unidimensional numbers are 6M IPv4 routes OR 3M IPv6 routes. This of course can vary depending on the prefixes distribution. All IPv4 and IPv6 routes are installed into a hierarchical LPM (Longest Prefix Match) database, apart from IPv6 host routes (IPv6 /128) which are installed into the CEM (Central Exact Match) database.
It's interesting to note unlike Q200, there is no hw-module profile or knob required to reach such scale.

**Info** The numbers provided and tested are without FIB compression and are raw LPM scale capabilities.
{: .notice--info}

# FIB Scale Testing Methodology
The methodology used for this 2024-2025 run is similar to the [previous tests]({{site.baseurl}}/tutorials/cisco-8000-fib-scale/) executed. An up-to date BGP routing table is used from the [RouteViews](https://www.routeviews.org/routeviews/) project. Latest forecast from [APNIC](https://blog.apnic.net/2025/01/06/bgp-in-2024/) are also used to simulate BGP growth. 

# Customer Case Study
To make this exercise even more realistic, following customer case study is presented in this article and a comparison is made with previous generation of Silicon One platforms.

- Customer profile: Satellite Internet Service Provider
- FIB profile: Internet routes + POD subscribers
- Current number of subscribers per POD: 300k
- Subscriber route profile: 1 x IPv4 /32 + 1 x IPv6 /64 + 1 x IPv6 /56 per subscriber

# Internet Baseline
Internet routes are first replayed on the DUT using routem (a Cisco internal tool to generate routes).

IPv4 configuration:
<div class="highlighter-rouge">
<pre class="highlight">
<code>
tme@8000-TME-Jump-Server:~/routem$ cat customerX-v4-to-8711-32FH-M
router bgp 65001
bgp_id 1.63.51.21
neighbor 1.63.51.70 remote-as 65537
neighbor 1.63.51.70 update-source 1.63.51.21
capability ipv4 unicast
capability refresh
capability 4bytes-as
<mark>replay file route-view-v4.cfg.198.58.198.252_AS1403_4byte ascii</mark>
</code>
</pre>
</div>

IPv6 configuration:
<div class="highlighter-rouge">
<pre class="highlight">
<code>
tme@8000-TME-Jump-Server:~/routem$ cat customerX-v6-to-8711-32FH-M
router bgp 65001
bgp_id 1.63.51.21
neighbor 2001:db8:1337:0:1:63:51:70 remote-as 65537
neighbor 2001:db8:1337:0:1:63:51:70 update-source 2001:db8:1337:0:1:63:51:21
capability ipv6 unicast
capability refresh
capability 4bytes-as
<mark>replay file route-view-v6.cfg.2606:6d00:eb0::252_AS1403_4byte ascii</mark>
tme@8000-TME-Jump-Server:~/routem$
</code>
</pre>
</div>

We can see following prefixes distribution corresponds to a true Internet feed:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh cef summary
Fri Oct 18 14:52:14.438 UTC

Router ID is 0.0.0.0

IP CEF with switching (Table Version 0) for node0_RP0_CPU0

  Load balancing: L4
  Tableid 0xe0000000 (0x9affeae8), Vrfid 0x60000000, Vrid 0x20000000, Flags 0x1019
  Vrfname default, Refcount 951072
  <mark>951032 routes</mark>, 0 protected, 0 reresolve, 2 unresolved (2 old, 0 new), 205423344 bytes
    951005 rib, 0 lsd, 22 aib, 0 internal, 2 interface, 4 special, 1 default routes
    Prefix masklen distribution:
        unicast: 21 /32, 582514 /24, 98490 /23, 111485 /22, 51552 /21, 43442 /20
                 24282 /19, 13634 /18, 8189 /17, 13157 /16, 2058 /15, 1171 /14
                 576 /13, 295 /12, 91 /11, 36 /10, 16 /9 , 16 /8
                 1 /0
</code>
</pre>
</div>



<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh cef ipv6 summary
Fri Oct 18 14:13:07.926 UTC

Router ID is 0.0.0.0

IP CEF with switching (Table Version 0) for node0_RP0_CPU0

  Load balancing: L4
  Tableid 0xe0800000 (0x9b663270), Vrfid 0x60000000, Vrid 0x20000000, Flags 0x1019
  Vrfname default, Refcount 208377
  <mark>208360 routes</mark>, 0 protected, 0 reresolve, 0 unresolved (0 old, 0 new), 45005760 bytes
    208350 rib, 0 lsd, 3 aib, 0 internal, 0 interface, 6 special, 1 default routes
    Prefix masklen distribution:
        unicast: 4 /128, 1 /64 , 96033 /48 , 5938 /47 , 4172 /46 , 2742 /45
                 19242 /44 , 1148 /43 , 2466 /42 , 1251 /41 , 20255 /40 , 1644 /39
                 2139 /38 , 1219 /37 , 7874 /36 , 1515 /35 , 4493 /34 , 4537 /33
                 25131 /32 , 334 /31 , 613 /30 , 5336 /29 , 150 /28 , 21 /27
                 20 /26 , 11 /25 , 33 /24 , 7 /23 , 6 /22 , 3 /21
                 13 /20 , 1 /19 , 1 /16 , 1 /10 , 1 /0
      multicast: 1 /128, 1 /104, 3 /16
</code>
</pre>
</div>

This matches current Internet table: roughly 1M IPv4 and 200k IPv6 prefixes. Same for distribution.

With those Internet prefixes, current FIB utilization is 19%:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh controllers npu resources lpmtcam location 0/RP0/CPU0
Fri Oct 18 14:53:04.786 UTC
HW Resource Information
    Name                            : lpm_tcam
    <mark>Asic Type                       : P100</mark>

NPU-0
OOR Summary
        Estimated Max Entries       : 100
        Red Threshold               : 95 %
        Yellow Threshold            : 80 %
        OOR State                   : Green
        High Water Mark             : 28
        High Water Mark Time        : 2024.Oct.18 13:48:25 UTC



Current Hardware Usage
    Name: lpm_tcam
        Estimated Max Entries       : 100
        <mark>Total In-Use                : 19       (19 %)</mark>
        OOR State                   : Green
        High Water Mark             : 28
        High Water Mark Time        : 2024.Oct.18 13:48:25 UTC


       Name: v4_lpm
           Total In-Use                : 951047
           High Water Mark             : 1249305
           High Water Mark Time        : 2024.Oct.18 08:56:24 UTC


       Name: v6_lpm
           Total In-Use                : 208365
           High Water Mark             : 808365
           High Water Mark Time        : 2024.Oct.18 13:48:25 UTC


       Name: v6_shortening_lpm
           Total In-Use                : 0
</code>
</pre>
</div>
# Subscribers’ routes addition

Extra routes associated to the 300k subscribers are advertised, meaning:
- 300k extra IPv4 /32 are advertised.
- 300k extra IPv6 /64 and /56 are advertised.

Giving following distribution:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh cef summary
Fri Oct 18 14:59:36.196 UTC

Router ID is 0.0.0.0

IP CEF with switching (Table Version 0) for node0_RP0_CPU0

  Load balancing: L4
  Tableid 0xe0000000 (0x9affeae8), Vrfid 0x60000000, Vrid 0x20000000, Flags 0x1019
  Vrfname default, Refcount 1251072
  1251032 routes, 0 protected, 0 reresolve, 2 unresolved (2 old, 0 new), 270223344 bytes
    1251005 rib, 0 lsd, 22 aib, 0 internal, 2 interface, 4 special, 1 default routes
    Prefix masklen distribution:
        unicast: <mark>300021 /32</mark>, 582514 /24, 98490 /23, 111485 /22, 51552 /21, 43442 /20
                 24282 /19, 13634 /18, 8189 /17, 13157 /16, 2058 /15, 1171 /14
                 576 /13, 295 /12, 91 /11, 36 /10, 16 /9 , 16 /8
                 1 /0
      broadcast: 4 /32
      multicast: 1 /24, 1 /4
</code>
</pre>
</div>
Similarly for IPv6, 2 x 300k are now observed:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh cef ipv6 summary
Fri Oct 18 15:00:00.232 UTC

Router ID is 0.0.0.0

IP CEF with switching (Table Version 0) for node0_RP0_CPU0

  Load balancing: L4
  Tableid 0xe0800000 (0x9b663270), Vrfid 0x60000000, Vrid 0x20000000, Flags 0x1019
  Vrfname default, Refcount 808377
  808360 routes, 0 protected, 0 reresolve, 0 unresolved (0 old, 0 new), 174605760 bytes
    808350 rib, 0 lsd, 3 aib, 0 internal, 0 interface, 6 special, 1 default routes
    Prefix masklen distribution:
        unicast: 4 /128, <mark>300001 /64 , 300000 /56</mark> , 96033 /48 , 5938 /47 , 4172 /46
                 2742 /45 , 19242 /44 , 1148 /43 , 2466 /42 , 1251 /41 , 20255 /40
                 1644 /39 , 2139 /38 , 1219 /37 , 7874 /36 , 1515 /35 , 4493 /34
                 4537 /33 , 25131 /32 , 334 /31 , 613 /30 , 5336 /29 , 150 /28
                 21 /27 , 20 /26 , 11 /25 , 33 /24 , 7 /23 , 6 /22
                 3 /21 , 13 /20 , 1 /19 , 1 /16 , 1 /10 , 1 /0
      multicast: 1 /128, 1 /104, 3 /16
</code>
</pre>
</div>
This FIB aims to represent a close and realistic representation of the customer role, with 300k subscribers. Under those conditions, FIB utilization is 30%:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh controllers npu resources lpmtcam location 0/RP0/CPU0
Fri Oct 18 15:00:32.340 UTC
HW Resource Information
    Name                            : lpm_tcam
    Asic Type                       : P100

NPU-0
OOR Summary
        Estimated Max Entries       : 100
        Red Threshold               : 95 %
        Yellow Threshold            : 80 %
        OOR State                   : Green
        High Water Mark             : 30
        High Water Mark Time        : 2024.Oct.18 14:57:55 UTC



Current Hardware Usage
    Name: lpm_tcam
        Estimated Max Entries       : 100
        <mark>Total In-Use                : 30       (30 %)</mark>
        OOR State                   : Green
        High Water Mark             : 30
        High Water Mark Time        : 2024.Oct.18 14:57:55 UTC


       Name: v4_lpm
           Total In-Use                : 1251047
           High Water Mark             : 1251047
           High Water Mark Time        : 2024.Oct.18 14:57:55 UTC


       Name: v6_lpm
           Total In-Use                : 808365
           High Water Mark             : 808365
           High Water Mark Time        : 2024.Oct.18 13:48:25 UTC


       Name: v6_shortening_lpm
           Total In-Use                : 0
</code>
</pre>
</div>

# Simulating 5Y Internet growth
Customer says it has plans to expand by 2025 and the number of subscribers will dramatically increase. This has several consequences for their FIB in the future:

- Rise of IPv4 /32
- Rise of IPv6 /64 and /56
- Rise of Internet table

For the later, we will simulate Internet table growth using RIPE growth projections and make following assumptions based on this study:

- IPv4 will decline: decision is to use same number of IPv4 routes as of today
- IPv6 will be continue to grow: we’ll use 535k IPv6 as an Internet target.

The extra 335k IPv6 routes are injected to follow a realistic Internet distribution:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
tme@8000-TME-Jump-Server:~/routem$ cat 2029_customerX-v6-to-8201-32FH
router bgp 65001
bgp_id 1.63.51.21
neighbor 2001:db8:1337:0:1:63:51:70 remote-as 65537
neighbor 2001:db8:1337:0:1:63:51:70 update-source 2001:db8:1337:0:1:63:51:21
capability ipv6 unicast
capability refresh
capability 4bytes-as
replay file IPv6-2023.2406:840:feed:2::1_AS3.12698_4byte ascii
<mark>network 1 2406:2d40::/64 300000
network 2 2a0d:3344::/56 300000
network 3 2001:8c00::/48 180000
network 4 2001:8d00::/32 49350
network 5 2001:8e00::/44 26320
network 6 2001:8f00::/40 23030
network 7 2001:9f00::/36 75000</mark>
</code>
</pre>
</div>

What this test gives is the 5Y Internet table, still with 300k subscribers: LPM utilization is 35%.

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh controllers npu resources lpmtcam location 0/RP0/CPU0
Fri Oct 18 15:02:49.136 UTC
HW Resource Information
    Name                            : lpm_tcam
    Asic Type                       : P100

NPU-0
OOR Summary
        Estimated Max Entries       : 100
        Red Threshold               : 95 %
        Yellow Threshold            : 80 %
        OOR State                   : Green
        High Water Mark             : 35
        High Water Mark Time        : 2024.Oct.18 15:02:25 UTC



Current Hardware Usage
    Name: lpm_tcam
        Estimated Max Entries       : 100
        <mark>Total In-Use                : 35       (35 %)</mark>
        OOR State                   : Green
        High Water Mark             : 35
        High Water Mark Time        : 2024.Oct.18 15:02:25 UTC


       Name: v4_lpm
           Total In-Use                : 1251047
           High Water Mark             : 1251047
           High Water Mark Time        : 2024.Oct.18 14:57:55 UTC


       Name: v6_lpm
           Total In-Use                : 1162054
           High Water Mark             : 1162054
           High Water Mark Time        : 2024.Oct.18 15:02:25 UTC


       Name: v6_shortening_lpm
           Total In-Use                : 0
</code>
</pre>
</div>

# Simulating subscribers growth

The next step is to simulate subscribers growth, from 300k to the maximum possible, using the same subscribers routes distribution (1 x IPv4 /32, 1 x IPv6 /64, 1 x IPv6 /56). Results are the following:

| FIB Profile                      | LPM Utilization |
|----------------------------------|-----------------|
| Internet 2029 + 300k subscribers | 35%             |
| Internet 2029 + 600k subscribers | 46%             |
| Internet 2029 + 900k subscribers | 59%             |
| Internet 2029 + 1.5M subscribers | 87%             |

While extra prefixes could be generated and platform could scale higher, the increase will be marginal and OOR Red level (95%) would be hit. Cisco doesn’t recommend running systems in production under those conditions.

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Saturn-8711-32FH-M#sh controllers npu resources lpmtcam location 0/RP0/CPU0
Fri Oct 18 15:34:39.205 UTC
HW Resource Information
    Name                            : lpm_tcam
    Asic Type                       : P100

NPU-0
OOR Summary
        Estimated Max Entries       : 100
        Red Threshold               : 95 %
        Yellow Threshold            : 80 %
        OOR State                   : Yellow
        OOR State Change Time       : 2024.Oct.18 15:29:27 UTC
        High Water Mark             : 87
        High Water Mark Time        : 2024.Oct.18 15:32:55 UTC



Current Hardware Usage
    Name: lpm_tcam
        Estimated Max Entries       : 100
        <mark>Total In-Use                : 87       (87 %)</mark>
        OOR State                   : Yellow
        OOR State Change Time       : 2024.Oct.18 15:29:27 UTC
        High Water Mark             : 87
        High Water Mark Time        : 2024.Oct.18 15:32:55 UTC


       <mark>Name: v4_lpm
           Total In-Use                : 2462049</mark>
           High Water Mark             : 2462049
           High Water Mark Time        : 2024.Oct.18 15:33:25 UTC


       <mark>Name: v6_lpm
           Total In-Use                : 3562054</mark>
           High Water Mark             : 3562054
           High Water Mark Time        : 2024.Oct.18 15:29:55 UTC


       Name: v6_shortening_lpm
           Total In-Use                : 0
</code>
</pre>
</div>

At 87% LPM utilization, the multi dimensional FIB scale is 2.5M IPv4 + 3.5M IPv6 prefixes.

# Silicon One Q200 and P100 comparison

To conclude this article, test was also executed on Cisco 8000 Q200 based platforms (with FIB expansion knob) to compare the improvement made between the 2 generations:

![q200-p100-comparison.png]({{site.baseurl}}/images/q200-p100-comparison.png)

For a similar number of subscribers (900k) and with Internet 2029 profile, Silicon One P100 consumes 31% less LPM resources compared to Q200.

The last graph shows how many subscribers can be attached without reaching a critical LPM utilization threshold (85%):

![q200-p100-max-subs.png]({{site.baseurl}}/images/q200-p100-max-subs.png)

For the same LPM utilization, Silicon One P100 can achieve 1.75x more subscribers compared to Silicon One Q200. 

# Conclusion
This article demonstrated the FIB scale capabilities of Cisco 8000 platforms based on Silicon One P100 ASIC. The results confirm the platform is future proof to accomodate not only Internet growth but also customers organic growth. Last, it covered a real-life use case and showed improvement made over previous generations of silicon.
