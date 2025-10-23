---
published: true
date: '2022-03-01 22:07 +0100'
title: >-
  Port Assignments on Cisco 8100/8200, Cisco 8700, Cisco 8608, and Cisco 8800
  Platforms (Part 1)
position: top
tags:
  - iosxr
  - Cisco 8000
  - Port
author: Chang Soo Lee
excerpt: >-
  This article will highlight how ports are allocated to IFG/Slice/NPU for each
  Cisco 8000 line card and systems (Part 1).
---
{% include toc icon="table" title="Port Assignments on Cisco 8100/8200, Cisco 8700, Cisco 8600, and Cisco 8800 Platforms (Part 1)"%} 


## Introduction 


The Cisco 8000 Series routers are built around network processors from the Cisco Silicon One family. To continue increasing performance, these network processors (NPUs) have an internal architecture that uses multiple hardware engines to process packets in parallel. The term we use for these internal engine groupings is “slices”. Note: don’t be confused with other platforms where an entire NPU is also called a “slice”. As semiconductor processes allow greater integration, we’ve seen systems like the Cisco 12000, where it took multiple large ASICs on a single linecard to implement the forwarding path. On the Cisco CRS-1, we moved from a few large ASICs to just a couple, to having multiple highly integrated NPUs on a single linecard. The ASR9k continued this integration with multiple NPUs on a card – here each NPU complex was referred to as a “slice”. On the Cisco 8000, each NPU has now integrated multiple forwarding engines onto a single chip along with the fabric interface as well. Better integration means fewer total chips in the forwarding path, with higher performance and lower power.
The majority of users will never need to know or care exactly how the front panel ports are mapped to these individual NPUs or which forwarding engines are used for a given interface. That said, understanding the port assignments can be useful in analyzing traffic flows, facilitating troubleshooting, and performance analysis. Let's review here how ports are allocated on each of the Cisco 8000 systems.

This first article explores port/IFG/Slice/NPU in Q100/Q200/Q200L/Q201L/Q202L/G100 based platforms. 

The following CLI command can be used on any Cisco 8000 system to display the assignments of physical ports to their corresponding NPU, slice, and IFG. (“IFG” stands for “interface group”, this is just another internal sub-block of the chip. There are two IFGs for each forwarding slice.

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:Cisco8201-32FH#<span style="background-color: #A0CFEC">sh controllers npu voq-usage interface all instance all loc 0/rp0/cpu0</span>
----------------------------------------------------------------------------------------
Node ID: 0/RP0/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------  
Hu0/0/0/0    78000118   0   4    1     4    704      0        local    100G      9
FH0/0/0/1    78000210   0   4    1    35   1008      0        local    400G      9
Hu0/0/0/2    78000140   0   4    1     9    712      0        local    100G      9
FH0/0/0/3    78000208   0   4    0    34   1000      0        local    400G      8
FH0/0/0/4    78000148   0   3    1    10    784      0        local    400G      7
FH0/0/0/5    78000200   0   4    0    33    992      0        local    400G      8
FH0/0/0/6    780001f8   0   5    0    32    976      0        local    400G     10
FH0/0/0/7    780001f0   0   5    1    31    960      0        local    400G     11
Hu0/0/0/8    78000138   0   5    1     8    768      0        local    100G     11
FH0/0/0/9    78000150   0   5    1    11    776      0        local    400G     11
FH0/0/0/10   78000158   0   5    0    12    800      0        local    400G     10
FH0/0/0/11   78000160   0   5    0    13    808      0        local    400G     10
Hu0/0/0/12   78000130   0   3    0     7    752      0        local    100G      6
FH0/0/0/13   78000168   0   3    1    14    792      0        local    400G      7
FH0/0/0/14   78000170   0   3    0    15    760      0        local    400G      6
FH0/0/0/15   78000178   0   3    0    16    816      0        local    400G      6
FH0/0/0/16   78000180   0   2    1    17    832      0        local    400G      5
FH0/0/0/17   78000188   0   2    1    18    840      0        local    400G      5
FH0/0/0/18   78000190   0   2    1    19    848      0        local    400G      5
FH0/0/0/19   78000198   0   2    0    20    864      0        local    400G      4
FH0/0/0/20   780001a0   0   0    1    21    744      0        local    400G      1
FH0/0/0/21   780001a8   0   0    1    22    880      0        local    400G      1
FH0/0/0/22   780001b0   0   0    0    23    896      0        local    400G      0
FH0/0/0/23   780001b8   0   0    0    24    904      0        local    400G      0
Hu0/0/0/24   78000128   0   0    1     6    736      0        local    100G      1
FH0/0/0/25   780001c0   0   0    0    25    912      0        local    400G      0
FH0/0/0/26   780001c8   0   2    0    26    872      0        local    400G      4
Hu0/0/0/27   78000120   0   1    1     5    720      0        local    100G      3
FH0/0/0/28   780001d0   0   1    0    27    928      0        local    400G      2
FH0/0/0/29   780001d8   0   1    1    28    728      0        local    400G      3
FH0/0/0/30   780001e0   0   1    0    29    936      0        local    400G      2
FH0/0/0/31   780001e8   0   1    0    30    944      0        local    400G      2  
</code>
</pre>
</div>  

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Cisco8608#<span style="background-color: #A0CFEC">show controllers npu voq-usage interface all instance all loc 0/rp0/cpu0</span>  
---------------------------------------------------------------------------
Node ID: 0/RP0/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed
             (hex)                                          type          
---------------------------------------------------------------------------
Hu0/1/0/15   f0000dc   0   5    0   320    632      0        local    100G
Hu0/1/0/14   f0000e4   0   5    0   312    624      0        local    100G
Hu0/1/0/13   f0000ec   0   5    0   304    616      0        local    100G
Hu0/1/0/12   f0000f4   0   5    0   296    608      0        local    100G
Hu0/1/0/11   f0000fc   0   5    0   288    600      0        local    100G
Hu0/1/0/10   f000104   0   5    0   280    592      0        local    100G
Hu0/1/0/9    f00010c   0   5    0   272    584      0        local    100G
Hu0/1/0/8    f000114   0   5    0   264    576      0        local    100G
Hu0/1/0/7    f00011c   0   4    1   256    568      0        local    100G
Hu0/1/0/6    f000124   0   4    1   248    560      0        local    100G
Hu0/1/0/5    f00012c   0   4    1   240    552      0        local    100G
Hu0/1/0/4    f000134   0   4    1   232    544      0        local    100G
Hu0/1/0/3    f00013c   0   4    1   224    536      0        local    100G
Hu0/1/0/2    f000144   0   4    1   216    528      0        local    100G
Hu0/1/0/1    f00014c   0   4    1   208    520      0        local    100G
Hu0/1/0/0    f0000d4   0   4    1   200    512      0        local    100G
FH0/2/0/3    f00017c   0   4    1   416    672      0        local    400G
FH0/2/0/2    f000184   0   4    0   408    664      0        local    400G
FH0/2/0/1    f00018c   0   4    0   400    656      0        local    400G
FH0/2/0/0    f000174   0   3    1   392    640      0        local    400G 
</code>
</pre>
</div>  


<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:88-LC0-34H14FH#<span style="background-color: #A0CFEC">show controllers npu voq-usage interface all instance all location 0/6/cpu0</span>    
-------------------------------------------------------------------
Node ID: 0/6/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow   VOQ    Port 
name         handle    <mark>#    #    #</mark>   Port  base  base   port   speed
             (hex)                                      type        
----------------------------------------------------------------------
Hu0/6/0/0    3000190   0   2    1  1156  28480      0 local   100G
Hu0/6/0/47   3000198   1   0    0  1344  28496      0 local   100G
Hu0/6/0/46   30001a0   1   0    1  1340  28512      0 local   100G
Hu0/6/0/45   30001a8   1   0    0  1336  28504      0 local   100G
Hu0/6/0/43   30001b0   1   0    1  1328  28520      0 local   100G
Hu0/6/0/42   30001b8   1   0    0  1324  28528      0 local   100G
Hu0/6/0/40   30001c0   1   1    0  1316  28544      0 local   100G
Hu0/6/0/39   30001c8   1   1    1  1312  28560      0 local   100G
Hu0/6/0/37   30001d0   1   1    0  1304  28552      0 local   100G
Hu0/6/0/36   30001d8   1   1    1  1300  28568      0 local   100G
Hu0/6/0/34   30001e0   1   2    0  1292  28576      0 local   100G
Hu0/6/0/33   30001e8   1   1    1  1288  28592      0 local   100G
Hu0/6/0/31   30001f0   1   2    0  1280  28584      0 local   100G
Hu0/6/0/30   30001f8   1   1    1  1276  28600      0 local   100G
Hu0/6/0/28   3000200   1   2    0  1268  28608      0 local   100G
Hu0/6/0/27   3000208   1   2    1  1264  28624      0 local   100G
Hu0/6/0/1    3000210   0   2    0  1160  28640      0 local   100G
Hu0/6/0/3    3000218   0   2    1  1168  28488      0 local   100G
Hu0/6/0/4    3000220   0   2    0  1172  28648      0 local   100G
Hu0/6/0/6    3000228   0   1    1  1180  28656      0 local   100G
Hu0/6/0/7    3000230   0   2    0  1184  28672      0 local   100G
Hu0/6/0/9    3000238   0   1    1  1192  28664      0 local   100G
Hu0/6/0/10   3000240   0   2    0  1196  28680      0 local   100G
Hu0/6/0/12   3000248   0   1    1  1204  28688      0 local   100G
Hu0/6/0/13   3000250   0   1    0  1208  28704      0 local   100G
Hu0/6/0/15   3000258   0   1    1  1216  28696      0 local   100G
Hu0/6/0/16   3000260   0   1    0  1220  28712      0 local   100G
Hu0/6/0/18   3000268   0   0    0  1228  28720      0 local   100G
Hu0/6/0/19   3000270   0   0    1  1232  28736      0 local   100G
Hu0/6/0/21   3000278   0   0    0  1240  28728      0 local   100G
Hu0/6/0/23   3000280   0   0    0  1248  28752      0 local   100G
Hu0/6/0/24   3000288   1   2    1  1252  28632      0 local   100G
Hu0/6/0/25   3000290   1   2    0  1256  28616      0 local   100G
Fo0/6/0/22   3000298   0   0    1  1244  28744      0 local    40G
FH0/6/0/44   30002a0   1   0    0  1332  28536      0 local   400G
FH0/6/0/41   30002a8   1   0    1  1320  28768      0 local   400G
FH0/6/0/38   30002b0   1   0    1  1308  28776      0 local   400G
FH0/6/0/35   30002b8   1   1    0  1296  28784      0 local   400G
FH0/6/0/32   30002c0   1   1    0  1284  28792      0 local   400G
FH0/6/0/29   30002c8   1   2    1  1272  28800      0 local   400G
FH0/6/0/26   30002d0   1   2    1  1260  28808      0 local   400G
FH0/6/0/20   30002d8   0   0    0  1236  28760      0 local   400G
FH0/6/0/17   30002e0   0   0    1  1224  28816      0 local   400G
FH0/6/0/14   30002e8   0   0    1  1212  28824      0 local   400G
FH0/6/0/11   30002f0   0   1    0  1200  28832      0 local   400G
FH0/6/0/8    30002f8   0   1    0  1188  28840      0 local   400G
FH0/6/0/5    3000300   0   2    1  1176  28848      0 local   400G
FH0/6/0/2    3000308   0   2    1  1164  28856      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28920      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28912      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28904      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28896      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28888      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28880      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28872      0 local   400G
FH0/6/0/2    3000308   0   0    0     0  28864      0 local   400G  
</code>
</pre>
</div>  


- NPU #: NPU number  
-- NPU number should be 0 for 8100/8200/8608 (8000 fixed & centralized systems)    
-- NPU number should be 0, 1, 2 for 88-LC0-36FH line card (not shown above)  
-- NPU number should be should be 0, 1 for 88-LC0-34H14FH line card        

- NPU core: Slice number  
-- Slice number should be between 0 and 5 for 8200/8608 (8000 fixed & centralized systems)   
-- Slice number should be between 0 and 7 for 8111-32EH (8100 fixed system)   
-- Slice number shoulbe be 0,1, 2 for 8800 Line cards     
- PP port: IFG number, should be 0 or 1 within slice  
{: .notice}

## Port allocation
### Cisco 8201
Cisco 8201 is made of a single Q100 NPU. 
![8201.png]({{site.baseurl}}/images/8201.png){: .full}  

The table below shows port assignment to different slices and IFG:

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|
| FH0/0/0/0  | 0/0/0         | FH0/0/0/14 | 0/3/6         | Hu0/0/0/28 | 0/1/3         |
| FH0/0/0/1  | 0/0/0         | FH0/0/0/15 | 0/3/6         | Hu0/0/0/29 | 0/1/2         |
| FH0/0/0/2  | 0/0/1         | FH0/0/0/16 | 0/2/5         | Hu0/0/0/30 | 0/0/1         |
| FH0/0/0/3  | 0/0/1         | FH0/0/0/17 | 0/2/5         | Hu0/0/0/31 | 0/0/0         |
| FH0/0/0/4  | 0/5/10        | FH0/0/0/18 | 0/2/4         | Hu0/0/0/32 | 0/5/11        |
| FH0/0/0/5  | 0/5/10        | FH0/0/0/19 | 0/2/4         | Hu0/0/0/33 | 0/5/10        |
| FH0/0/0/6  | 0/5/11        | FH0/0/0/20 | 0/1/3         | Hu0/0/0/34 | 0/4/9         |
| FH0/0/0/7  | 0/5/11        | FH0/0/0/21 | 0/1/3         | Hu0/0/0/35 | 0/4/8         |
| FH0/0/0/8  | 0/4/9         | FH0/0/0/22 | 0/1/2         |            |               |
| FH0/0/0/9  | 0/4/9         | FH0/0/0/23 | 0/1/2         |            |               |
| FH0/0/0/10 | 0/4/8         | Hu0/0/0/24 | 0/3/7         |            |               |
| FH0/0/0/11 | 0/4/8         | Hu0/0/0/25 | 0/3/6         |            |               |
| FH0/0/0/12 | 0/3/7         | Hu0/0/0/26 | 0/2/5         |            |               |
| FH0/0/0/13 | 0/3/7         | Hu0/0/0/27 | 0/2/4         |            |               |

### Cisco 8202
Cisco 8202 is made of a single Q100 NPU.  
![8202.png]({{site.baseurl}}/images/8202.png)   

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|------------|---------------|------------|---------------|
| Hu0/0/0/0  | 0/0/1         | Hu0/0/0/15 | 0/0/1         | Hu0/0/0/30 | 0/2/5         | Hu0/0/0/45 | 0/4/9         | Hu0/0/0/60 | 0/3/6         |
| Hu0/0/0/1  | 0/0/1         | Hu0/0/0/16 | 0/1/3         | Hu0/0/0/31 | 0/2/5         | Hu0/0/0/46 | 0/0/0         | Hu0/0/0/61 | 0/2/5         |
| Hu0/0/0/2  | 0/0/1         | Hu0/0/0/17 | 0/2/4         | Hu0/0/0/32 | 0/2/4         | Hu0/0/0/47 | 0/5/11        | Hu0/0/0/62 | 0/4/8         |
| Hu0/0/0/3  | 0/0/1         | Hu0/0/0/18 | 0/0/1         | Hu0/0/0/33 | 0/2/4         | FH0/0/0/48 | 0/5/11        | Hu0/0/0/63 | 0/3/7         |
| Hu0/0/0/4  | 0/0/0         | Hu0/0/0/19 | 0/1/2         | Hu0/0/0/34 | 0/2/4         | FH0/0/0/49 | 0/5/11        | Hu0/0/0/64 | 0/1/3         |
| Hu0/0/0/5  | 0/0/0         | Hu0/0/0/20 | 0/2/4         | Hu0/0/0/35 | 0/2/4         | FH0/0/0/50 | 0/5/10        | Hu0/0/0/65 | 0/1/3         |
| Hu0/0/0/6  | 0/0/0         | Hu0/0/0/21 | 0/2/4         | Hu0/0/0/36 | 0/1/2         | FH0/0/0/51 | 0/5/10        | Hu0/0/0/66 | 0/1/3         |
| Hu0/0/0/7  | 0/0/0         | Hu0/0/0/22 | 0/2/4         | Hu0/0/0/37 | 0/1/2         | FH0/0/0/52 | 0/4/9         | Hu0/0/0/67 | 0/1/3         |
| Hu0/0/0/8  | 0/0/0         | Hu0/0/0/23 | 0/2/4         | Hu0/0/0/38 | 0/1/2         | FH0/0/0/53 | 0/4/9         | Hu0/0/0/68 | 0/1/3         |
| Hu0/0/0/9  | 0/0/0         | Hu0/0/0/24 | 0/2/5         | Hu0/0/0/39 | 0/1/2         | FH0/0/0/54 | 0/4/8         | Hu0/0/0/69 | 0/1/3         |
| Hu0/0/0/10 | 0/0/0         | Hu0/0/0/25 | 0/2/5         | Hu0/0/0/40 | 0/1/2         | FH0/0/0/55 | 0/4/8         | Hu0/0/0/70 | 0/1/3         |
| Hu0/0/0/11 | 0/0/0         | Hu0/0/0/26 | 0/2/5         | Hu0/0/0/41 | 0/1/2         | FH0/0/0/56 | 0/3/7         | Hu0/0/0/71 | 0/1/3         |
| Hu0/0/0/12 | 0/0/1         | Hu0/0/0/27 | 0/2/5         | Hu0/0/0/42 | 0/1/2         | FH0/0/0/57 | 0/3/7         |            |               |
| Hu0/0/0/13 | 0/0/1         | Hu0/0/0/28 | 0/2/5         | Hu0/0/0/43 | 0/1/2         | FH0/0/0/58 | 0/3/6         |            |               |
| Hu0/0/0/14 | 0/0/1         | Hu0/0/0/29 | 0/2/5         | Hu0/0/0/44 | 0/5/10        | FH0/0/0/59 | 0/3/6         |            |               |  


### Cisco 8201-32FH
Cisco 8201-32FH is made of a single Q200 NPU.  
![8201-32FH.png]({{site.baseurl}}/images/8201-32FH.png)  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|
| FH0/0/0/0  | 0/4/9         | FH0/0/0/16 | 0/2/5         |
| FH0/0/0/1  | 0/4/9         | FH0/0/0/17 | 0/2/5         |
| FH0/0/0/2  | 0/4/9         | FH0/0/0/18 | 0/2/5         |
| FH0/0/0/3  | 0/4/8         | FH0/0/0/19 | 0/2/4         |
| FH0/0/0/4  | 0/3/7         | FH0/0/0/20 | 0/0/1         |
| FH0/0/0/5  | 0/4/8         | FH0/0/0/21 | 0/0/1         |
| FH0/0/0/6  | 0/5/10        | FH0/0/0/22 | 0/0/0         |
| FH0/0/0/7  | 0/5/11        | FH0/0/0/23 | 0/0/0         |
| FH0/0/0/8  | 0/5/11        | FH0/0/0/24 | 0/0/1         |
| FH0/0/0/9  | 0/5/11        | FH0/0/0/25 | 0/0/0         |
| FH0/0/0/10 | 0/5/10        | FH0/0/0/26 | 0/2/4         |
| FH0/0/0/11 | 0/5/10        | FH0/0/0/27 | 0/1/3         |
| FH0/0/0/12 | 0/3/6         | FH0/0/0/28 | 0/1/2         |
| FH0/0/0/13 | 0/3/7         | FH0/0/0/29 | 0/1/3         |
| FH0/0/0/14 | 0/3/6         | FH0/0/0/30 | 0/1/2         |
| FH0/0/0/15 | 0/3/6         | FH0/0/0/31 | 0/1/2         |

### Cisco 8202-32FH-M
Cisco 8202-32FH-M is made of a single Q200 NPU. It supports MACsec on all ports.  
![8202-32FH-M.png]({{site.baseurl}}/images/8202-32FH-M.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|
| FH0/0/0/0  | 0/5/11        | FH0/0/0/16 | 0/1/3         |
| FH0/0/0/1  | 0/5/11        | FH0/0/0/17 | 0/2/4         |
| FH0/0/0/2  | 0/4/9         | FH0/0/0/18 | 0/1/3         |
| FH0/0/0/3  | 0/4/9         | FH0/0/0/19 | 0/1/2         |
| FH0/0/0/4  | 0/4/9         | FH0/0/0/20 | 0/1/2         |
| FH0/0/0/5  | 0/4/8         | FH0/0/0/21 | 0/1/2         |
| FH0/0/0/6  | 0/3/7         | FH0/0/0/22 | 0/0/0         |
| FH0/0/0/7  | 0/4/8         | FH0/0/0/23 | 0/0/0         |
| FH0/0/0/8  | 0/5/10        | FH0/0/0/24 | 0/3/6         |
| FH0/0/0/9  | 0/5/10        | FH0/0/0/25 | 0/3/7         |
| FH0/0/0/10 | 0/5/10        | FH0/0/0/26 | 0/3/6         |
| FH0/0/0/11 | 0/5/11        | FH0/0/0/27 | 0/3/6         |
| FH0/0/0/12 | 0/0/0         | FH0/0/0/28 | 0/2/5         |
| FH0/0/0/13 | 0/0/1         | FH0/0/0/29 | 0/2/5         |
| FH0/0/0/14 | 0/0/1         | FH0/0/0/30 | 0/2/5         |
| FH0/0/0/15 | 0/0/1         | FH0/0/0/31 | 0/2/4         |


### Cisco 8201-24H8FH
Cisco 8201-24H8FH is made of a single Q200 NPU. This system can support 24 ports 100 GbE and 8 ports 400 GbE.  

![Rhine photo.png]({{site.baseurl}}/images/Rhine photo.png){: .full}  

![Rhine.png]({{site.baseurl}}/images/Rhine.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|
| FH0/0/0/0  | 0/4/9         | FH0/0/0/14 | 0/3/6         | Hu0/0/0/28 | 0/1/2         |
| Hu0/0/0/1  | 0/4/9         | Hu0/0/0/15 | 0/3/6         | Hu0/0/0/29 | 0/1/3         |
| FH0/0/0/2  | 0/4/9         | Hu0/0/0/16 | 0/2/5         | Hu0/0/0/30 | 0/1/2         |
| Hu0/0/0/3  | 0/4/8         | Hu0/0/0/17 | 0/2/5         | Hu0/0/0/31 | 0/1/2         |
| FH0/0/0/4  | 0/3/7         | Hu0/0/0/18 | 0/2/5         |            |               |
| Hu0/0/0/5  | 0/4/8         | Hu0/0/0/19 | 0/2/4         |            |               |
| FH0/0/0/6  | 0/5/10        | Hu0/0/0/20 | 0/0/1         |            |               |
| Hu0/0/0/7  | 0/5/11        | Hu0/0/0/21 | 0/0/1         |            |               |
| FH0/0/0/8  | 0/5/11        | Hu0/0/0/22 | 0/0/0         |            |               |
| Hu0/0/0/9  | 0/5/11        | Hu0/0/0/23 | 0/0/0         |            |               |
| FH0/0/0/10 | 0/5/10        | Hu0/0/0/24 | 0/0/1         |            |               |
| Hu0/0/0/11 | 0/5/10        | Hu0/0/0/25 | 0/0/0         |            |               |
| FH0/0/0/12 | 0/3/6         | Hu0/0/0/26 | 0/2/4         |            |               |
| Hu0/0/0/13 | 0/3/7         | Hu0/0/0/27 | 0/1/3         |            |               |  


### Cisco 8101-32FH
Cisco 8101-32FH is made of a single Q200L NPU.  
![8101-32FH.png]({{site.baseurl}}/images/8101-32FH.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|
| FH0/0/0/0  | 0/4/9         | FH0/0/0/16 | 0/2/5         |
| FH0/0/0/1  | 0/4/9         | FH0/0/0/17 | 0/2/5         |
| FH0/0/0/2  | 0/4/9         | FH0/0/0/18 | 0/2/5         |
| FH0/0/0/3  | 0/4/8         | FH0/0/0/19 | 0/2/4         |
| FH0/0/0/4  | 0/3/7         | FH0/0/0/20 | 0/0/1         |
| FH0/0/0/5  | 0/4/8         | FH0/0/0/21 | 0/0/1         |
| FH0/0/0/6  | 0/5/10        | FH0/0/0/22 | 0/0/0         |
| FH0/0/0/7  | 0/5/11        | FH0/0/0/23 | 0/0/0         |
| FH0/0/0/8  | 0/5/11        | FH0/0/0/24 | 0/0/1         |
| FH0/0/0/9  | 0/5/11        | FH0/0/0/25 | 0/0/0         |
| FH0/0/0/10 | 0/5/10        | FH0/0/0/26 | 0/2/4         |
| FH0/0/0/11 | 0/5/10        | FH0/0/0/27 | 0/1/3         |
| FH0/0/0/12 | 0/3/6         | FH0/0/0/28 | 0/1/2         |
| FH0/0/0/13 | 0/3/7         | FH0/0/0/29 | 0/1/3         |
| FH0/0/0/14 | 0/3/6         | FH0/0/0/30 | 0/1/2         |
| FH0/0/0/15 | 0/3/6         | FH0/0/0/31 | 0/1/2         |

### Cisco 8101-32H
Cisco 8101-32H is made of a single Q202L NPU.  
![8101-32H.png]({{site.baseurl}}/images/8101-32H.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|
| Hu0/0/0/0  | 0/2/5         | Hu0/0/0/16 | 0/1/2         |
| Hu0/0/0/1  | 0/2/5         | Hu0/0/0/17 | 0/1/2         |
| Hu0/0/0/2  | 0/2/5         | Hu0/0/0/18 | 0/1/2         |
| Hu0/0/0/3  | 0/2/5         | Hu0/0/0/19 | 0/1/2         |
| Hu0/0/0/4  | 0/2/5         | Hu0/0/0/20 | 0/0/1         |
| Hu0/0/0/5  | 0/2/5         | Hu0/0/0/21 | 0/0/1         |
| Hu0/0/0/6  | 0/2/4         | Hu0/0/0/22 | 0/0/1         |
| Hu0/0/0/7  | 0/2/4         | Hu0/0/0/23 | 0/0/1         |
| Hu0/0/0/8  | 0/2/4         | Hu0/0/0/24 | 0/0/1         |
| Hu0/0/0/9  | 0/2/4         | Hu0/0/0/25 | 0/0/1         |
| Hu0/0/0/10 | 0/1/3         | Hu0/0/0/26 | 0/0/0         |
| Hu0/0/0/11 | 0/1/3         | Hu0/0/0/27 | 0/0/0         |
| Hu0/0/0/12 | 0/1/3         | Hu0/0/0/28 | 0/0/0         |
| Hu0/0/0/13 | 0/1/3         | Hu0/0/0/29 | 0/0/0         |
| Hu0/0/0/14 | 0/1/2         | Hu0/0/0/30 | 0/0/0         |
| Hu0/0/0/15 | 0/1/2         | Hu0/0/0/31 | 0/0/0         |

### Cisco 8102-64H
Cisco 8102-64H is made of a single Q201L NPU.  
![8111-32EH.png]({{site.baseurl}}/images/8111-32EH.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|------------|---------------|------------|---------------|
| Hu0/0/0/0  | 0/4/9         | Hu0/0/0/14 | 0/5/11        | Hu0/0/0/28 | 0/3/6         | Hu0/0/0/42 | 0/0/1         | Hu0/0/0/56 | 0/1/2         |
| Hu0/0/0/1  | 0/4/9         | Hu0/0/0/15 | 0/5/11        | Hu0/0/0/29 | 0/3/6         | Hu0/0/0/43 | 0/0/1         | Hu0/0/0/57 | 0/1/2         |
| Hu0/0/0/2  | 0/4/9         | Hu0/0/0/16 | 0/5/11        | Hu0/0/0/30 | 0/3/6         | Hu0/0/0/44 | 0/0/0         | Hu0/0/0/58 | 0/1/3         |
| Hu0/0/0/3  | 0/4/9         | Hu0/0/0/17 | 0/5/11        | Hu0/0/0/31 | 0/3/6         | Hu0/0/0/45 | 0/0/0         | Hu0/0/0/59 | 0/1/3         |
| Hu0/0/0/4  | 0/4/9         | Hu0/0/0/18 | 0/5/11        | Hu0/0/0/32 | 0/2/5         | Hu0/0/0/46 | 0/0/0         | Hu0/0/0/60 | 0/1/2         |
| Hu0/0/0/5  | 0/4/9         | Hu0/0/0/19 | 0/5/11        | Hu0/0/0/33 | 0/2/5         | Hu0/0/0/47 | 0/0/0         | Hu0/0/0/61 | 0/1/2         |
| Hu0/0/0/6  | 0/4/8         | Hu0/0/0/20 | 0/5/10        | Hu0/0/0/34 | 0/2/5         | Hu0/0/0/48 | 0/0/1         | Hu0/0/0/62 | 0/1/2         |
| Hu0/0/0/7  | 0/4/8         | Hu0/0/0/21 | 0/5/10        | Hu0/0/0/35 | 0/2/5         | Hu0/0/0/49 | 0/0/1         | Hu0/0/0/63 | 0/1/2         |
| Hu0/0/0/8  | 0/3/7         | Hu0/0/0/22 | 0/5/10        | Hu0/0/0/36 | 0/2/5         | Hu0/0/0/50 | 0/0/0         |            |               |
| Hu0/0/0/9  | 0/3/7         | Hu0/0/0/23 | 0/5/10        | Hu0/0/0/37 | 0/2/5         | Hu0/0/0/51 | 0/0/0         |            |               |
| Hu0/0/0/10 | 0/4/8         | Hu0/0/0/24 | 0/3/6         | Hu0/0/0/38 | 0/2/4         | Hu0/0/0/52 | 0/2/4         |            |               |
| Hu0/0/0/11 | 0/4/8         | Hu0/0/0/25 | 0/3/6         | Hu0/0/0/39 | 0/2/4         | Hu0/0/0/53 | 0/2/4         |            |               |
| Hu0/0/0/12 | 0/5/10        | Hu0/0/0/26 | 0/3/7         | Hu0/0/0/40 | 0/0/1         | Hu0/0/0/54 | 0/1/3         |            |               |
| Hu0/0/0/13 | 0/5/10        | Hu0/0/0/27 | 0/3/7         | Hu0/0/0/41 | 0/0/1         | Hu0/0/0/55 | 0/1/3         |            |               |

### Cisco 8111-32EH
Cisco 8111-32EH is made of a single G100 NPU.  
![Screenshot 2023-07-08 at 11.50.31 AM.png]({{site.baseurl}}/images/Screenshot 2023-07-08 at 11.50.31 AM.png){: .full}  

| Interface                                              | NPU/Slice/IFG | Interface                                              | NPU/Slice/IFG |
|--------------------------------------------------------|---------------|--------------------------------------------------------|---------------|
| FH0/0/0/0/0, FH0/0/0/0/1, FH0/0/0/1/0, FH0/0/0/1/1     | 0/4/8         | FH0/0/0/16/0, FH0/0/0/16/1, FH0/0/0/17/0, FH0/0/0/17/1 | 0/1/3         |
| FH0/0/0/2/0, FH0/0/0/2/1, FH0/0/0/3/0, FH0/0/0/3/1     | 0/4/9         | FH0/0/0/18/0, FH0/0/0/18/1, FH0/0/0/19/0, FH0/0/0/19/1 | 0/1/2         |
| FH0/0/0/4/0, FH0/0/0/4/1, FH0/0/0/5/0, FH0/0/0/5/1     | 0/5/11        | FH0/0/0/20/0, FH0/0/0/20/1, FH0/0/0/21/0, FH0/0/0/21/1 | 0/0/0         |
| FH0/0/0/6/0, FH0/0/0/6/1, FH0/0/0/7/0, FH0/0/0/7/1     | 0/5/10        | FH0/0/0/22/0, FH0/0/0/22/1, FH0/0/0/23/0, FH0/0/0/23/1 | 0/0/1         |
| FH0/0/0/8/0, FH0/0/0/8/1, FH0/0/0/9/0, FH0/0/0/9/1     | 0/3/7         | FH0/0/0/24/0, FH0/0/0/24/1, FH0/0/0/25/0, FH0/0/0/25/1 | 0/6/12        |
| FH0/0/0/10/0, FH0/0/0/10/1, FH0/0/0/11/0, FH0/0/0/11/1 | 0/3/6         | FH0/0/0/26/0, FH0/0/0/26/1, FH0/0/0/27/0, FH0/0/0/27/1 | 0/6/13        |
| FH0/0/0/12/0, FH0/0/0/12/1, FH0/0/0/13/0, FH0/0/0/13/1 | 0/2/4         | FH0/0/0/28/0, FH0/0/0/28/1, FH0/0/0/29/0, FH0/0/0/29/1 | 0/7/15        |
| FH0/0/0/14/0, FH0/0/0/14/1, FH0/0/0/15/0, FH0/0/0/15/1 | 0/2/5         | FH0/0/0/30/0, FH0/0/0/30/1, FH0/0/0/31/0, FH0/0/0/31/1 | 0/7/14        |  

### Cisco 8608  
The Cisco 8608 router is a part of Cisco’s Centralized Architecture based chassis design. Centralized systems have redundant Route Processors (RP w/CPU), redundant Switch cards (SCs w/Cisco Silicon One Q200 NPU) and in-service replaceable modular port adapters (MPA).  

![Screenshot 2023-09-13 at 2.02.52 PM.png]({{site.baseurl}}/images/Screenshot 2023-09-13 at 2.02.52 PM.png){: .full}  

| MPA                    | NPU/Slice/IFG        | MPA                    | NPU/Slice/IFG        |
|------------------------|----------------------|------------------------|----------------------|
| MPA 0 (Up to 1.6 Tbps) | 0/5/10,  0/5/11      | MPA 4 (Up to 1.6 Tbps) | 0/2/4,  0/2/5        |
| MPA 1 (Up to 1.6 Tbps) | 0/4/9,  0/5/10       | MPA 5 (Up to 1.6 Tbps) | 0/1/2,  0/1/3, 0/2/4 |
| MPA 2 (Up to 1.6 Tbps) | 0/3/7,  0/4/8, 0/4/9 | MPA 6 (Up to 1.6 Tbps) | 0/0/1,  0/1/2        |
| MPA 3 (Up to 1.6 Tbps) | 0/3/6,  0/3/7        | MPA 7 (Up to 1.6 Tbps) | 0/0/0,  0/0/1        |  

### 8800-LC-36FH
There are four Q100 NPUs on these line cards.  
![LC36-Q100.png]({{site.baseurl}}/images/LC36-Q100.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|
| FH0/x/0/0  | 2/2/5         | FH0/x/0/14 | 1/1/3         | FH0/x/0/28 | 3/1/2         |
| FH0/x/0/1  | 0/2/5         | FH0/x/0/15 | 1/2/4         | FH0/x/0/29 | 3/1/2         |
| FH0/x/0/2  | 2/2/5         | FH0/x/0/16 | 1/1/2         | FH0/x/0/30 | 0/0/1         |
| FH0/x/0/3  | 0/2/4         | FH0/x/0/17 | 1/1/2         | FH0/x/0/31 | 3/0/1         |
| FH0/x/0/4  | 0/2/4         | FH0/x/0/18 | 0/1/3         | FH0/x/0/32 | 3/0/0         |
| FH0/x/0/5  | 2/2/4         | FH0/x/0/19 | 1/0/1         | FH0/x/0/33 | 3/0/1         |
| FH0/x/0/6  | 2/1/2         | FH0/x/0/20 | 1/0/1         | FH0/x/0/34 | 0/0/0         |
| FH0/x/0/7  | 2/1/3         | FH0/x/0/21 | 0/1/2         | FH0/x/0/35 | 0/0/0         |
| FH0/x/0/8  | 2/0/1         | FH0/x/0/22 | 1/0/0         |            |               |
| FH0/x/0/9  | 2/1/2         | FH0/x/0/23 | 0/1/2         |            |               |
| FH0/x/0/10 | 2/0/0         | FH0/x/0/24 | 3/2/5         |            |               |
| FH0/x/0/11 | 2/0/1         | FH0/x/0/25 | 3/2/5         |            |               |
| FH0/x/0/12 | 1/2/5         | FH0/x/0/26 | 3/1/3         |            |               |
| FH0/x/0/13 | 1/2/5         | FH0/x/0/27 | 3/2/4         |            |               |

**Note**: x is slot number in Cisco 8800 router   

### 88-LC0-36FH-M and 88-LC0-36FH
There are three Q200 NPUs on these line cards.  
88-LC0-36FH-M and 88-LC0-36FH are identical in all ways except the former can support MACSec and the latter cannot.

![VG.png]({{site.baseurl}}/images/VG.png)         
<b>88-LC0-36FH-M</b>  
{: .text-center}  
 
![Lancer.png]({{site.baseurl}}/images/Lancer.png)        
<b>88-LC0-36FH</b>  
{: .text-center}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|
| FH0/x/0/0  | 0/2/5         | FH0/x/0/14 | 1/2/4         | FH0/x/0/28 | 2/1/3         |
| FH0/x/0/1  | 0/2/5         | FH0/x/0/15 | 1/2/4         | FH0/x/0/29 | 2/1/3         |
| FH0/x/0/2  | 0/2/4         | FH0/x/0/16 | 1/1/3         | FH0/x/0/30 | 2/1/2         |
| FH0/x/0/3  | 0/2/4         | FH0/x/0/17 | 1/1/3         | FH0/x/0/31 | 2/1/2         |
| FH0/x/0/4  | 0/1/3         | FH0/x/0/18 | 1/1/2         | FH0/x/0/32 | 2/0/1         |
| FH0/x/0/5  | 0/1/3         | FH0/x/0/19 | 1/1/2         | FH0/x/0/33 | 2/0/1         |
| FH0/x/0/6  | 0/1/2         | FH0/x/0/20 | 1/0/1         | FH0/x/0/34 | 2/0/0         |
| FH0/x/0/7  | 0/1/2         | FH0/x/0/21 | 1/0/1         | FH0/x/0/35 | 2/0/0         |
| FH0/x/0/8  | 0/0/1         | FH0/x/0/22 | 1/0/0         |            |               |
| FH0/x/0/9  | 0/0/1         | FH0/x/0/23 | 1/0/0         |            |               |
| FH0/x/0/10 | 0/0/0         | FH0/x/0/24 | 2/2/5         |            |               |
| FH0/x/0/11 | 0/0/0         | FH0/x/0/25 | 2/2/5         |            |               |
| FH0/x/0/12 | 1/2/5         | FH0/x/0/26 | 2/2/4         |            |               |
| FH0/x/0/13 | 1/2/5         | FH0/x/0/27 | 2/2/4         |            |               |

**Note**: x is slot number in Cisco 8800 router  

### 8800-LC-48H 
There are two Q100 NPUs on these line cards.
![LC48.png]({{site.baseurl}}/images/LC48.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|------------|---------------|
| Hu0/x/0/0  | 0/1/2         | Hu0/x/0/14 | 1/2/5         | Hu0/x/0/28 | 1/1/3         | Hu0/x/0/42 | 0/0/0         |
| Hu0/x/0/1  | 0/2/5         | Hu0/x/0/15 | 0/1/3         | Hu0/x/0/29 | 1/1/3         | Hu0/x/0/43 | 1/0/0         |
| FH0/x/0/2  | 0/2/5         | Hu0/x/0/16 | 1/2/5         | Hu0/x/0/30 | 0/0/1         | Hu0/x/0/44 | 1/0/0         |
| Hu0/x/0/3  | 0/1/2         | Hu0/x/0/17 | 1/2/5         | Hu0/x/0/31 | 1/1/2         | Hu0/x/0/45 | 0/0/0         |
| Hu0/x/0/4  | 0/2/5         | Hu0/x/0/18 | 0/1/3         | Hu0/x/0/32 | 1/1/2         | Hu0/x/0/46 | 1/0/0         |
| Hu0/x/0/5  | 0/2/5         | Hu0/x/0/19 | 1/2/4         | Hu0/x/0/33 | 0/0/1         | Hu0/x/0/47 | 1/0/0         |
| Hu0/x/0/6  | 0/1/2         | Hu0/x/0/20 | 1/2/4         | Hu0/x/0/34 | 1/1/2         |            |               |
| Hu0/x/0/7  | 0/2/4         | Hu0/x/0/21 | 0/1/3         | Hu0/x/0/35 | 1/1/2         |            |               |
| Hu0/x/0/8  | 0/2/4         | Hu0/x/0/22 | 1/2/4         | Hu0/x/0/36 | 0/0/0         |            |               |
| Hu0/x/0/9  | 0/1/2         | Hu0/x/0/23 | 1/2/4         | Hu0/x/0/37 | 1/0/1         |            |               |
| Hu0/x/0/10 | 0/2/4         | Hu0/x/0/24 | 0/0/1         | Hu0/x/0/38 | 1/0/1         |            |               |
| Hu0/x/0/11 | 0/2/4         | Hu0/x/0/25 | 1/1/3         | Hu0/x/0/39 | 0/0/0         |            |               |
| Hu0/x/0/12 | 0/1/3         | Hu0/x/0/26 | 1/1/3         | Hu0/x/0/40 | 1/0/1         |            |               |
| Hu0/x/0/13 | 1/2/5         | Hu0/x/0/27 | 0/0/1         | Hu0/x/0/41 | 1/0/1         |            |               |

**Note**: x is slot number in Cisco 8800 router  

### 88-LC0-34H14FH
There are two Q200 NPUs on these line cards.  
![LC48Q200.png]({{site.baseurl}}/images/LC48Q200.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|------------|---------------|
| Hu0/x/0/0  | 0/2/5         | FH0/x/0/14 | 0/0/1         | Hu0/x/0/28 | 1/2/4         | Hu0/x/0/42 | 1/0/0         |
| Hu0/x/0/1  | 0/2/4         | Hu0/x/0/15 | 0/1/3         | FH0/x/0/29 | 1/2/5         | Hu0/x/0/43 | 1/0/1         |
| FH0/x/0/2  | 0/2/5         | Hu0/x/0/16 | 0/1/2         | Hu0/x/0/30 | 1/1/3         | FH0/x/0/44 | 1/0/0         |
| Hu0/x/0/3  | 0/2/5         | FH0/x/0/17 | 0/0/1         | Hu0/x/0/31 | 1/2/4         | Hu0/x/0/45 | 1/0/0         |
| Hu0/x/0/4  | 0/2/4         | Hu0/x/0/18 | 0/0/0         | FH0/x/0/32 | 1/1/2         | Hu0/x/0/46 | 1/0/1         |
| FH0/x/0/5  | 0/2/5         | Hu0/x/0/19 | 0/0/1         | Hu0/x/0/33 | 1/1/3         | Hu0/x/0/47 | 1/0/0         |
| Hu0/x/0/6  | 0/1/3         | FH0/x/0/20 | 0/0/0         | Hu0/x/0/34 | 1/2/4         |            |               |
| Hu0/x/0/7  | 0/2/4         | Hu0/x/0/21 | 0/0/0         | FH0/x/0/35 | 1/1/2         |            |               |
| FH0/x/0/8  | 0/1/2         | Hu0/x/0/22 | 0/0/1         | Hu0/x/0/36 | 1/1/3         |            |               |
| Hu0/x/0/9  | 0/1/3         | Hu0/x/0/23 | 0/0/0         | Hu0/x/0/37 | 1/1/2         |            |               |
| Hu0/x/0/10 | 0/2/4         | Hu0/x/0/24 | 1/2/5         | FH0/x/0/38 | 1/0/1         |            |               |
| FH0/x/0/11 | 0/1/2         | Hu0/x/0/25 | 1/2/4         | Hu0/x/0/39 | 1/1/3         |            |               |
| Hu0/x/0/12 | 0/1/3         | FH0/x/0/26 | 1/2/5         | Hu0/x/0/40 | 1/1/2         |            |               |
| Hu0/x/0/13 | 0/1/2         | Hu0/x/0/27 | 1/2/5         | FH0/x/0/41 | 1/0/1         |            |               |  

**Note**: x is slot number in Cisco 8800 router  

 
You can find the second part of this post here: [Cisco 8000 Port assignment (Part 2)]({{site.url}}/8000/tutorials/cisco-8000-port-assignment-part-2/)  


## Modification History  

| Version | Data     | Author(s)     | Comments            |
|---------|----------|---------------|---------------------|
| 1       | March-22 | Chang Soo Lee | Initial Publication |
| 2       | April-22 | Chang Soo Lee | Added 8202-32FH-M   |
| 3       | July-22  | Chang Soo Lee | Added 8201-24H8FH   |
| 4       | April-23 | Chang Soo Lee | Added 8111-32EH     |
| 5       | July-23  | Chang Soo Lee | Upgraded to high resolution     |  
| 6       | September-13  | Chang Soo Lee | Added 8608     |
