---
published: true
date: '2025-05-16 16:46 +0200'
title: Cisco 8000 Packet Generator
author: Frederic Cuiller
position: hidden
tags:
  - Cisco 8000
  - Silicon One
  - Packet Generator
  - Scapy
excerpt: >-
  This article aims to document Cisco 8000 and Silicon One packet generator
  functionality. The built-in packet generator can be used for troubleshooting
  purposes or to perform network diagnostics without the need of external
  traffic generator.
---
# Introduction
This article aims to document Cisco 8000 and Silicon One packet generator functionality introduced with IOS XR 24.2.11. The built-in packet generator can be used for troubleshooting purposes or to perform network diagnostics without the need of external traffic generator.

# Implementation

Cisco 8000 Packet Generator leverages Silicon One ASIC NPU Host. This component is typically used for OAM features (TWAMP, SRv6 IPM, BFD, CFM etc.). NPU host is built with its own Network Processor Engine (NPE) and can access all NPU slices.

It provides true hardware assisted packet generation without involving device CPU. Performance are the following:

- It supports a rate of 13.7Mpps
- On Silicon One Q100 and Q200, maximum supported packet size is 608B allowing to generate 66.5 Gbps of traffic per NPU
- On Silicon One K100 nd P100, maximum supported packet size is 4000B allowing to generate 438Gbps per NPU

# Configuration and Verification
The CCO [configuration](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/system-monitoring/25xx/configuration/guide/b-system-monitoring-cg-cisco8k-25xx/m-configuring-built-in-traffic-generator.html) and [command reference](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/system-monitoring/b-system-monitoring-cr-cisco8k/tgen-commands.html#diagnostic-packet-generator-create) guides list the multiple options available and how to configure them.
For this article demo, a simple unidirectional traffic flow is generated between 2 routers using IPv6 traffic for an infinite period of time.

The CLI prompts provides several examples:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:t26#diagnostic packet-generator create P2P rate 100 duration infinite packet ?
  packet  General:
              Packet can be a hex string or scapy script. If provided as a hex string, ensure it is a valid byte string (even length, in hexadecimal).
              Packet provided to CLI cannot go over 255 characters, for longer packets please use the "file" option.
          ingress:
              If user packet doesn't have Ether header, it's added with src mac 00:00:00:00:00:01, dst
              mac as the ingress interface mac (not supported if packet is in hex format).
                  Note: the packet must be injected from the LC where the interface resides.
              Example 1 - inject IPv6 packets
                  diagnostic packet-generator create T1 packet IPv6(src="sip",dst="dip",hlim=64)/Raw(load="f"*200) ingress interface BE115.15 location 0/0/CPU0
          egress:
              Example 2 - Send packets from Hu0/0/0/10
                  diagnostic packet-generator create T2 packet Ether(src="smac",dst="dmac")/IP(src="sip",dst="dip",ttl=64)/Raw(load="f"*200) egress interface HundredGigE0/0/0/10 lo
cation 0/RP0/CPU0
</code>
</pre>
</div>

First, the packet must be created/forged and following information are entered:

- SRC and DST MAC
- SRC and DST IPv6
- Payload is raw 900 x ‘f’
- Rate is 100 pps
- Egress interface is HundredGigE0/0/0/17
- Egress mode: skips RxPP lookups and TxPP encaps, sends the packet as-it-is on egress slice and interface
- Injected from a fixed system (i.e single NPU, location 0/RP0/CPU0)

[Scapy expression](https://scapy.readthedocs.io/en/latest/usage.html) is used to forge/built the packet. If the packet description is too complex (i.e too many parameters to enter inside the CLI), user must instead use a file (pcap file with .pcap suffix or a text file with scapy script or hex string).

<div class="highlighter-rouge">
<pre class="highlight">
<code>
diagnostic packet-generator create P2P rate 100 duration infinite packet Ether(src="a0:bc:6f:75:2e:14",dst="40:06:d5:05:28:60")/IPv6(src="2001:db8:1337:2604::26",dst="2001:db8:1337:2604::04",hlim=64)/Raw(load="f"*900) egress interface HundredGigE0/0/0/17 location 0/rp0/CPU0
Fri May 16 07:16:22.347 PDT
<mark>OK</mark>
RP/0/RP0/CPU0:t26#
</code>
</pre>
</div>

System returns ‘OK’ is the Scapy expression is correct. If not, an error is returned (Invalid Scapy expression).  

After configuration, the flow can be verified:
<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:t26#sh diagnostic packet-generator status P2P location 0/RP0/CPU0
Fri May 16 07:19:10.925 PDT
0/RP0/CPU0:
Name           Run_State    Type    Capture  Set_Rate(pps)  Applied_Rate(pps)  Duration(sec)  TC  Phy_Interface  NPU  Slice  IFG  Packets        Bytes
================================================================================================================================================================
P2P            <mark>Stopped</mark>      Egress  False    100            100                Infinite       0   Hu0/0/0/17     0    0      0    0              0
----------------------------------------------------------------------------------------------------------------------------------------------------------------
Packet Details:
###[ Ethernet ]###
  dst       = 40:06:d5:05:28:60
  src       = a0:bc:6f:75:2e:14
  type      = IPv6
###[ IPv6 ]###
     version   = 6
     tc        = 0
     fl        = 0
     plen      = 900
     nh        = No Next Header
     hlim      = 64
     src       = 2001:db8:1337:2604::26
     dst       = 2001:db8:1337:2604::4
###[ Raw ]###
        load      = 'ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'



RP/0/RP0/CPU0:t26#
</code>
</pre>
</div>

Then it can be launched:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:t26#diagnostic packet-generator start P2P location 0/RP0/CPU0
Fri May 16 07:20:34.230 PDT
<mark>OK</mark>
</code>
</pre>
</div>

Verification is done again:
- the flow is running
- number of packets and bytes increase

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:t26#sh diagnostic packet-generator status P2P location 0/RP0/CPU0
Fri May 16 07:20:55.812 PDT
0/RP0/CPU0:
Name           Run_State    Type    Capture  Set_Rate(pps)  Applied_Rate(pps)  Duration(sec)  TC  Phy_Interface  NPU  Slice  IFG  Packets        Bytes
================================================================================================================================================================
P2P            <mark>Running</mark>      Egress  False    100            100                Infinite       0   Hu0/0/0/17     0    0      0    <mark>2256</mark>           <mark>2240208</mark>
----------------------------------------------------------------------------------------------------------------------------------------------------------------
Packet Details:
###[ Ethernet ]###
  dst       = 40:06:d5:05:28:60
  src       = a0:bc:6f:75:2e:14
  type      = IPv6
###[ IPv6 ]###
     version   = 6
     tc        = 0
     fl        = 0
     plen      = 900
     nh        = No Next Header
     hlim      = 64
     src       = 2001:db8:1337:2604::26
     dst       = 2001:db8:1337:2604::4
###[ Raw ]###
        load      = 'ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'



RP/0/RP0/CPU0:t26#
</code>
</pre>
</div>

Moreover we can observe packets are leaving the interface at the expected rate of 100pps checking interface statistics:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:t26#sh int Hu0/0/0/17
Fri May 16 07:23:58.312 PDT
HundredGigE0/0/0/17 is up, line protocol is up
  Interface state transitions: 9
  Hardware is HundredGigE, address is a0bc.6f75.2e14 (bia a0bc.6f75.2e14)
  Description: to T9
  Internet address is Unknown
  MTU 9200 bytes, BW 100000000 Kbit (Max: 100000000 Kbit)
     reliability 255/255, txload 0/255, rxload 0/255
  Encapsulation ARPA,
  Full-duplex, 100000Mb/s, 100GBASE-SR-BD, link type is force-up
  output flow control is off, input flow control is off
  Carrier delay (up) is 200 msec
  loopback not set,
  Last link flapped 04:58:48
  Last input 00:00:00, output 00:00:00
  Last clearing of "show interface" counters never
  30 second input rate 30000 bits/sec, 33 packets/sec
  <mark>30 second output rate 771000 bits/sec, 106 packets/sec</mark>
     1519550 packets input, 171035060 bytes, 0 total input drops
     0 drops for unrecognized upper-level protocol
     Received 0 broadcast packets, 1155054 multicast packets
              0 runts, 0 giants, 0 throttles, 0 parity
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     460366 packets output, 208884462 bytes, 0 total output drops
     Output 0 broadcast packets, 31144 multicast packets
     0 output errors, 0 underruns, 0 applique, 0 resets
     0 output buffer failures, 0 output buffers swapped out
     9 carrier transitions
</code>
</pre>
</div>

If a parameter must be changed (e.g rate), the flow must be deleted and created again:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:t26#diagnostic packet-generator create P2P rate max duration infinite packet Ether(src="a0:bc:6f:75:2e:14",dst="40:06:d5:05:28:60")/IPv6(src="2001:db8:1337:2604::26",dst="2$
Fri May 16 07:24:51.154 PDT
<mark>Operation failed, packet generator already exists.</mark>
</code>
</pre>
</div>

Here, asking the system to generate as much as traffic as possible (rate max):

<div class="highlighter-rouge">
<pre class="highlight">
<code>
diagnostic packet-generator create P2P rate max duration infinite packet Ether(src="a0:bc:6f:75:2e:14",dst="40:06:d5:05:28:60")/IPv6(src="2001:db8:1337:2604::26",dst="2001:db8:1337:2604::04",hlim=64)/Raw(load="f"*900) egress interface HundredGigE0/0/0/17 location 0/rp0/CPU0

RP/0/RP0/CPU0:t26#sh diagnostic packet-generator status P2P location 0/RP0/CPU0
Fri May 16 07:27:27.385 PDT
0/RP0/CPU0:
Name           Run_State    Type    Capture  Set_Rate(pps)  Applied_Rate(pps)  Duration(sec)  TC  Phy_Interface  NPU  Slice  IFG  Packets        Bytes
================================================================================================================================================================
P2P            Running      Egress  False    <mark>Max            12292793</mark>           Infinite       0   Hu0/0/0/17     0    0      0    106890310      106142077830
----------------------------------------------------------------------------------------------------------------------------------------------------------------
Packet Details:
###[ Ethernet ]###
  dst       = 40:06:d5:05:28:60
  src       = a0:bc:6f:75:2e:14
  type      = IPv6
###[ IPv6 ]###
     version   = 6
     tc        = 0
     fl        = 0
     plen      = 900
     nh        = No Next Header
     hlim      = 64
     src       = 2001:db8:1337:2604::26
     dst       = 2001:db8:1337:2604::4
###[ Raw ]###
        load      = 'ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'



RP/0/RP0/CPU0:t26#
</code>
</pre>
</div>

# Conclusion & Use cases
This feature has been recently used inside one customer network during Early Field Trial. This customer was testing 800G ZR+ optics and wanted to validate the long-distance link between 2 x Cisco 8000 routers. It was not possible to reroute customer traffic nor interconnecting a regular traffic generator. Cisco 8000 embedded traffic generator functionality was leveraged to validate correct long distance transmission across the 2 endpoints. Other customers are using this feature during their provisionning routing to validate massive ECMP fabrics and make sure links are error-free before going into production. Another use case is to craft specific packets to test Segment Routing policies. Unlimited possibilities! 

