---
published: true
date: '2024-09-26 10:43 -0700'
title: >-
  Port Assignments on Cisco 8100/8200, Cisco 8700, Cisco 8608, and Cisco 8800
  Platforms (Part 2)
author: Chang Soo Lee
excerpt: >-
  This article will highlight how ports are allocated to IFG/Slice/NPU for each
  Cisco 8000 line card and systems (Part 2).
tags:
  - iosxr
  - Cisco 8000
  - Port
position: top
---
{% include toc icon="table" title="Port Assignments on Cisco 8100/8200, Cisco 8700, Cisco 8600, and Cisco 8800 Platforms (Part 2)"%}  


## Introduction 


The Cisco 8000 Series routers are built around network processors from the Cisco Silicon One family. To continue increasing performance, these network processors (NPUs) have an internal architecture that uses multiple hardware engines to process packets in parallel. The term we use for these internal engine groupings is “slices”. Note: don’t be confused with other platforms where an entire NPU is also called a “slice”. As semiconductor processes allow greater integration, we’ve seen systems like the Cisco 12000, where it took multiple large ASICs on a single linecard to implement the forwarding path. On the Cisco CRS-1, we moved from a few large ASICs to just a couple, to having multiple highly integrated NPUs on a single linecard. The ASR9k continued this integration with multiple NPUs on a card – here each NPU complex was referred to as a “slice”. On the Cisco 8000, each NPU has now integrated multiple forwarding engines onto a single chip along with the fabric interface as well. Better integration means fewer total chips in the forwarding path, with higher performance and lower power.
The majority of users will never need to know or care exactly how the front panel ports are mapped to these individual NPUs or which forwarding engines are used for a given interface. That said, understanding the port assignments can be useful in analyzing traffic flows, facilitating troubleshooting, and performance analysis. Let's review here how ports are allocated on each of the Cisco 8000 systems.

This second article explores port/IFG/Slice/NPU in P100 based platforms.  

The following CLI command can be used on any Cisco 8000 system to display the assignments of physical ports to their corresponding NPU, slice, and IFG. (“IFG” stands for “interface group”, this is just another internal sub-block of the chip. There are two IFGs for each forwarding slice.  

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:8212-48FH-M#<span style="background-color: #A0CFEC">sh controllers npu voq-usage interface all instance all loc 0/rp0/cpu0</span>  
----------------------------------------------------------------------------------------
Node ID: 0/RP0/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------
FH0/0/0/1    780001b0   0   0    1     5    720      0        local    400G      1
FH0/0/0/2    780001b8   0   5    1     6    712      0        local    400G     11
FH0/0/0/3    780001c0   0   0    1     7    728      0        local    400G      1
FH0/0/0/4    780001c8   0   5    1     8    736      0        local    400G     11
FH0/0/0/5    780001d0   0   0    1     9    752      0        local    400G      1
FH0/0/0/6    780001d8   0   5    1    10    744      0        local    400G     11
FH0/0/0/7    780001e0   0   0    1    11    760      0        local    400G      1
FH0/0/0/9    780001f8   0   1    0    14    784      0        local    400G      2
FH0/0/0/11   78000200   0   1    0    15    792      0        local    400G      2
FH0/0/0/12   78000208   0   4    0    16    800      0        local    400G      8
FH0/0/0/13   78000210   0   1    0    17    816      0        local    400G      2
FH0/0/0/14   78000218   0   4    0    18    808      0        local    400G      8
FH0/0/0/15   78000220   0   1    0    19    824      0        local    400G      2
FH0/0/0/16   78000228   0   4    1    20    832      0        local    400G      9
FH0/0/0/17   78000230   0   1    1    21    848      0        local    400G      3
FH0/0/0/19   78000240   0   1    1    23    856      0        local    400G      3
FH0/0/0/20   78000248   0   4    1    24    864      0        local    400G      9
FH0/0/0/21   78000250   0   1    1    25    880      0        local    400G      3
FH0/0/0/22   78000258   0   4    1    26    872      0        local    400G      9
FH0/0/0/24   78000268   0   5    0    28    896      0        local    400G     10
FH0/0/0/25   78000270   0   0    0    29    912      0        local    400G      0
FH0/0/0/26   78000278   0   5    0    30    904      0        local    400G     10
FH0/0/0/27   78000280   0   0    0    31    920      0        local    400G      0
FH0/0/0/29   78000290   0   0    0    33    944      0        local    400G      0
FH0/0/0/31   780002a0   0   0    0    35    952      0        local    400G      0
FH0/0/0/34   780002b8   0   3    1    38    976      0        local    400G      7
FH0/0/0/35   780002c0   0   3    1    39    984      0        local    400G      7
FH0/0/0/36   780002c8   0   3    0    40    992      0        local    400G      6
FH0/0/0/37   780002d0   0   3    0    41   1000      0        local    400G      6
FH0/0/0/38   780002d8   0   3    0    42   1008      0        local    400G      6
FH0/0/0/39   780002e0   0   3    0    43   1016      0        local    400G      6
FH0/0/0/41   780002f0   0   2    1    45   1032      0        local    400G      5
FH0/0/0/42   780002f8   0   2    1    46   1040      0        local    400G      5
FH0/0/0/43   78000300   0   2    1    47   1048      0        local    400G      5
FH0/0/0/47   78000320   0   2    0    51   1080      0        local    400G      4
Hu0/0/0/0/0  78000388   0   5    1    52    704      0        local    100G     11
Hu0/0/0/46/1 78000390   0   2    0    53   1072      0        local    100G      4
Hu0/0/0/46/2 78000398   0   2    0    54   1088      0        local    100G      4
Hu0/0/0/46/3 780003a0   0   2    0    55   1096      0        local    100G      4
Hu0/0/0/46/0 780003c8   0   2    0    60   1104      0        local    100G      4
Hu0/0/0/0/1  780003d0   0   5    1    61   1120      0        local    100G     11
Hu0/0/0/0/2  780003d8   0   5    1    62   1128      0        local    100G     11
Hu0/0/0/0/3  780003e0   0   5    1    63   1136      0        local    100G     11
FH0/0/0/32/0 78000640   0   3    1    64    960      0        local    400G      7
FH0/0/0/32/1 78000648   0   3    1    65    968      0        local    400G      7
FH0/0/0/45   78000798   0   2    0    66   1112      0        local    400G      4
FH0/0/0/44   780007a8   0   2    0    67   1056      0        local    400G      4
FH0/0/0/8    78000920   0   4    0    68    768      0        local    400G      8
FH0/0/0/10   78000928   0   4    0    69    776      0        local    400G      8
<mark>Hu0/0/0/28/0</mark> 780010d8   0   5    0    70   1192      0        local    100G     10
<mark>Hu0/0/0/28/1</mark> 780010a0   0   5    0    71    928      0        local    100G     10
<mark>Hu0/0/0/28/2</mark> 780010a8   0   5    0    72    936      0        local    100G     10
<mark>Hu0/0/0/28/3</mark> 780010b0   0   5    0    73   1152      0        local    100G     10
<mark>Hu0/0/0/28/4</mark> 780010b8   0   5    0    74   1160      0        local    100G     10
<mark>Hu0/0/0/28/5</mark> 780010c0   0   5    0    75   1168      0        local    100G     10
<mark>Hu0/0/0/28/6</mark> 780010c8   0   5    0    76   1176      0        local    100G     10
<mark>Hu0/0/0/28/7</mark> 780010d0   0   5    0    77   1184      0        local    100G     10
FH0/0/0/40   78015510   0   2    1    44   1024      0        local    400G      5
Hu0/0/0/23   78015870   0   1    1    27    888      0        local    100G      3
FH0/0/0/18   78016210   0   4    1    22    840      0        local    400G      9  
</code>
</pre>
</div>   


<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:8711-32FH-M#<span style="background-color: #A0CFEC">sh controllers npu voq-usage interface all instance all loc 0/rp0/cpu0</span>  
----------------------------------------------------------------------------------------
Node ID: 0/RP0/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------
Hu0/0/0/0    78000138   0   3    1     4    576      0        local    100G      7
Hu0/0/0/30   78000140   0   0    0     5    592      0        local    100G      0
Hu0/0/0/24   78000148   0   0    1     6    608      0        local    100G      1
Hu0/0/0/19   78000150   0   1    1     7    624      0        local    100G      3
Hu0/0/0/18   78000158   0   1    1     8    632      0        local    100G      3
Hu0/0/0/16   78000160   0   1    1     9    640      0        local    100G      3
Hu0/0/0/14/1 78000168   0   2    0    10    656      0        local    100G      4
Hu0/0/0/14/0 78000170   0   2    0    11    664      0        local    100G      4
Hu0/0/0/12   78000178   0   2    0    12    672      0        local    100G      4
Hu0/0/0/1    78000180   0   3    1    13    584      0        local    100G      7
FH0/0/0/6    78000188   0   3    0    14    688      0        local    400G      6
FH0/0/0/13   78000190   0   2    0    15    680      0        local    400G      4
FH0/0/0/15   78000198   0   2    0    16    704      0        local    400G      4
FH0/0/0/17   780001a0   0   1    1    17    648      0        local    400G      3
<mark>FH0/0/0/20/0</mark> 780001a8   0   1    0    18    720      0        local    400G      2
<mark>FH0/0/0/20/1</mark> 780001b0   0   1    0    19    728      0        local    400G      2
FH0/0/0/22   780001b8   0   1    0    20    736      0        local    400G      2
FH0/0/0/23   780001c0   0   1    0    21    744      0        local    400G      2
FH0/0/0/25   780001c8   0   0    1    22    616      0        local    400G      1
FH0/0/0/26   780001d0   0   0    1    23    752      0        local    400G      1
FH0/0/0/27   780001d8   0   0    1    24    760      0        local    400G      1
FH0/0/0/28   780001e0   0   0    0    25    600      0        local    400G      0
FH0/0/0/29   780001e8   0   0    0    26    768      0        local    400G      0
FH0/0/0/31   780001f0   0   0    0    27    776      0        local    400G      0
FH0/0/0/11   780001f8   0   2    1    28    784      0        local    400G      5
FH0/0/0/10   78000200   0   2    1    29    792      0        local    400G      5
FH0/0/0/9    78000208   0   2    1    30    800      0        local    400G      5
FH0/0/0/8    78000210   0   2    1    31    808      0        local    400G      5
FH0/0/0/7    78000218   0   3    0    32    696      0        local    400G      6
FH0/0/0/5    78000220   0   3    0    33    816      0        local    400G      6
FH0/0/0/4    78000228   0   3    0    34    824      0        local    400G      6
FH0/0/0/3    78000230   0   3    1    35    832      0        local    400G      7
FH0/0/0/2    78000238   0   3    1    36    840      0        local    400G      7  
</code>
</pre>
</div>  

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:8712-MOD-M#<span style="background-color: #A0CFEC">show controllers npu voq-usage interface all instance all location 0/rp0/cpu0</span>   

----------------------------------------------------------------------------------------
Node ID: 0/RP0/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------
FH0/1/0/0    780000b4   0   1    0     4    544      0        local    400G      2
FH0/1/0/1    780000bc   0   1    0     5    552      0        local    400G      2
FH0/0/0/2    780000c4   0   0    0     6    592      0        local    400G      0
FH0/0/0/1    780000cc   0   0    0     7    584      0        local    400G      0
FH0/0/0/0    780000d4   0   0    0     8    576      0        local    400G      0
FH0/1/0/3    780000dc   0   1    0     9    568      0        local    400G      2
FH0/1/0/2    780000e4   0   1    0    10    560      0        local    400G      2
Hu0/0/0/3    780000ec   0   0    0    11    600      0        local    100G      0

</code>
</pre>
</div>

<div class="highlighter-rouge">
<pre class="highlight">
<code>  

RP/0/RP0/CPU0:Cisco8808-88-LC1-36EH#<span style="background-color: #A0CFEC">show controllers npu voq-usage interface all instance all location 0/5/CPU0</span>  
----------------------------------------------------------------------------------------
Node ID: 0/5/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------
FH0/5/0/35/1 140002e0   2   0    0     4  24064      0        local    400G      0
FH0/5/0/35/0 140002e8   2   0    0     5  24072      0        local    400G      0
FH0/5/0/34/1 140002f0   2   0    0     6  24080      0        local    400G      0
FH0/5/0/34/0 140002f8   2   0    1     7  24096      0        local    400G      1
FH0/5/0/32/1 14000300   2   1    0     8  24112      0        local    400G      2
FH0/5/0/32/0 14000308   2   1    0     9  24120      0        local    400G      2
FH0/5/0/31/1 14000310   2   1    1    10  24128      0        local    400G      3
FH0/5/0/31/0 14000318   2   1    1    11  24136      0        local    400G      3
FH0/5/0/30/1 14000320   2   1    0    12  24144      0        local    400G      2
FH0/5/0/30/0 14000328   2   1    1    13  24160      0        local    400G      3
FH0/5/0/0/0  14000330   0   2    0    14  24176      0        local    400G      4
FH0/5/0/0/1  14000338   0   2    1    15  24192      0        local    400G      5
FH0/5/0/1/0  14000340   0   2    0    16  24184      0        local    400G      4
FH0/5/0/1/1  14000348   0   2    0    17  24208      0        local    400G      4
FH0/5/0/2/0  14000350   0   2    1    18  24200      0        local    400G      5
FH0/5/0/2/1  14000358   0   2    1    19  24224      0        local    400G      5
FH0/5/0/4/0  14000360   0   1    1    20  24240      0        local    400G      3
FH0/5/0/4/1  14000368   0   1    0    21  24256      0        local    400G      2
FH0/5/0/8/0  14000370   1   2    1    22  24272      0        local    400G      5
FH0/5/0/8/1  14000378   1   2    1    23  24280      0        local    400G      5
FH0/5/0/9/0  14000380   0   0    0    24  24288      0        local    400G      0
FH0/5/0/9/1  14000388   0   0    0    25  24296      0        local    400G      0
FH0/5/0/11/0 14000390   1   2    0    26  24304      0        local    400G      4
FH0/5/0/11/1 14000398   1   2    0    27  24312      0        local    400G      4
FH0/5/0/12/0 140003a0   1   1    1    28  24320      0        local    400G      3
FH0/5/0/12/1 140003a8   1   1    0    29  24336      0        local    400G      2
FH0/5/0/15/0 140003b0   1   1    0    30  24344      0        local    400G      2
FH0/5/0/15/1 140003b8   1   1    0    31  24352      0        local    400G      2
FH0/5/0/16/0 140003c0   1   0    1    32  24368      0        local    400G      1
FH0/5/0/16/1 140003c8   1   0    0    33  24384      0        local    400G      0
FH0/5/0/17/0 140003d0   1   0    1    34  24376      0        local    400G      1
FH0/5/0/17/1 140003d8   1   0    1    35  24400      0        local    400G      1
FH0/5/0/19/0 140003e0   2   2    1    36  24416      0        local    400G      5
FH0/5/0/19/1 140003e8   2   2    1    37  24424      0        local    400G      5
FH0/5/0/20/0 140003f0   3   2    1    38  24432      0        local    400G      5
FH0/5/0/20/1 140003f8   3   2    1    39  24440      0        local    400G      5
FH0/5/0/21/0 14000400   2   2    0    40  24448      0        local    400G      4
FH0/5/0/21/1 14000408   2   2    0    41  24456      0        local    400G      4
FH0/5/0/22/0 14000410   3   2    0    42  24464      0        local    400G      4
FH0/5/0/22/1 14000418   3   2    1    43  24480      0        local    400G      5
FH0/5/0/23/0 14000420   3   2    0    44  24472      0        local    400G      4
FH0/5/0/23/1 14000428   3   2    0    45  24496      0        local    400G      4
FH0/5/0/24/0 14000430   3   1    1    46  24512      0        local    400G      3
FH0/5/0/24/1 14000438   3   1    0    47  24528      0        local    400G      2
FH0/5/0/25/0 14000440   3   1    1    48  24520      0        local    400G      3
FH0/5/0/25/1 14000448   3   1    1    49  24544      0        local    400G      3
FH0/5/0/26/0 14000450   3   1    0    50  24536      0        local    400G      2
FH0/5/0/26/1 14000458   3   1    0    51  24560      0        local    400G      2
FH0/5/0/27/0 14000460   3   0    1    52  24576      0        local    400G      1
FH0/5/0/27/1 14000468   3   0    1    53  24584      0        local    400G      1
FH0/5/0/28/0 14000470   3   0    1    54  24592      0        local    400G      1
FH0/5/0/28/1 14000478   3   0    0    55  24608      0        local    400G      0
FH0/5/0/29/0 14000480   3   0    0    56  24616      0        local    400G      0
FH0/5/0/29/1 14000488   3   0    0    57  24624      0        local    400G      0
Hu0/5/0/3    14000490   0   1    1    58  24248      0        local    100G      3
Hu0/5/0/33   14000498   2   0    1    59  24104      0        local    100G      1
Hu0/5/0/18   140004a0   2   2    1    60  24640      0        local    100G      5
Hu0/5/0/14   140004a8   1   0    0    61  24392      0        local    100G      0
Hu0/5/0/13   140004b0   1   1    1    62  24328      0        local    100G      3
Hu0/5/0/10   140004b8   1   2    1    63  24656      0        local    100G      5
Hu0/5/0/7    140004c0   0   0    1    64  24672      0        local    100G      1
Hu0/5/0/6    140004c8   0   0    1    65  24680      0        local    100G      1
Hu0/5/0/5    140004d0   0   1    0    66  24264      0        local    100G      2  
</code>
</pre>
</div>    

<div class="highlighter-rouge">
<pre class="highlight">
<code>  
RP/0/RP0/CPU0:Cisco8808-88-LC1-12TH24FH-E#<span style="background-color: #A0CFEC">show controllers npu voq-usage interface all instance all location 0/7/CPU0</span>
----------------------------------------------------------------------------------------
Node ID: 0/7/CPU0
Intf         Intf     <mark>NPU Slice IFG</mark>  Sys   VOQ   Flow       VOQ      Port       Global 
name         handle    <mark>#    #    #</mark>   Port  base  base       port     speed      IFG #
             (hex)                                          type                
----------------------------------------------------------------------------------------
Hu0/7/0/0/0  1c0001f0   0   2    0     4  40064      0    <mark>etm_local</mark>    100G      4
Hu0/7/0/0/0  1c0001f0   0   2    0     4  33344      0   <mark>etm_global</mark>    100G      4
Hu0/7/0/0/1  1c0001f8   0   2    1     5  52816      0    etm_local    100G      5
Hu0/7/0/0/1  1c0001f8   0   2    1     5  33360      0   etm_global    100G      5
Hu0/7/0/4/0  1c000200   0   1    1     6  52800      0    etm_local    100G      3
Hu0/7/0/4/0  1c000200   0   1    1     6  33376      0   etm_global    100G      3
Hu0/7/0/4/1  1c000208   0   1    0     7  40080      0    etm_local    100G      2
Hu0/7/0/4/1  1c000208   0   1    0     7  33392      0   etm_global    100G      2
Hu0/7/0/12/0 1c000230   1   1    1    12  52800      0    etm_local    100G      3
Hu0/7/0/12/0 1c000230   1   1    1    12  33472      0   etm_global    100G      3
Hu0/7/0/12/1 1c000238   1   1    0    13  40080      0    etm_local    100G      2
Hu0/7/0/12/1 1c000238   1   1    0    13  33488      0   etm_global    100G      2
Hu0/7/0/16/0 1c000240   1   0    1    14  52800      0    etm_local    100G      1
Hu0/7/0/16/0 1c000240   1   0    1    14  33504      0   etm_global    100G      1
Hu0/7/0/16/1 1c000248   1   0    0    15  40080      0    etm_local    100G      0
Hu0/7/0/16/1 1c000248   1   0    0    15  33520      0   etm_global    100G      0
Hu0/7/0/18/0 1c000250   2   2    1    16  52800      0    etm_local    100G      5
Hu0/7/0/18/0 1c000250   2   2    1    16  33536      0   etm_global    100G      5
Hu0/7/0/18/1 1c000258   2   2    0    17  40080      0    etm_local    100G      4
Hu0/7/0/18/1 1c000258   2   2    0    17  33552      0   etm_global    100G      4
Hu0/7/0/22/0 1c000260   3   2    0    18  40064      0    etm_local    100G      4
Hu0/7/0/22/0 1c000260   3   2    0    18  33568      0   etm_global    100G      4
Hu0/7/0/22/1 1c000268   3   2    1    19  52816      0    etm_local    100G      5
Hu0/7/0/22/1 1c000268   3   2    1    19  33584      0   etm_global    100G      5
Hu0/7/0/24/0 1c000270   3   1    1    20  52800      0    etm_local    100G      3
Hu0/7/0/24/0 1c000270   3   1    1    20  33600      0   etm_global    100G      3
Hu0/7/0/24/1 1c000278   3   1    0    21  40080      0    etm_local    100G      2
Hu0/7/0/24/1 1c000278   3   1    0    21  33616      0   etm_global    100G      2
Hu0/7/0/28/0 1c000280   3   0    1    22  52800      0    etm_local    100G      1
Hu0/7/0/28/0 1c000280   3   0    1    22  33632      0   etm_global    100G      1
Hu0/7/0/28/1 1c000288   3   0    0    23  40080      0    etm_local    100G      0
Hu0/7/0/28/1 1c000288   3   0    0    23  33648      0   etm_global    100G      0
Hu0/7/0/30/0 1c000290   2   1    1    24  52800      0    etm_local    100G      3
Hu0/7/0/30/0 1c000290   2   1    1    24  33664      0   etm_global    100G      3
Hu0/7/0/30/1 1c000298   2   1    0    25  40080      0    etm_local    100G      2
Hu0/7/0/30/1 1c000298   2   1    0    25  33680      0   etm_global    100G      2
Hu0/7/0/34/0 1c0002a0   2   0    1    26  52800      0    etm_local    100G      1
Hu0/7/0/34/0 1c0002a0   2   0    1    26  33696      0   etm_global    100G      1
Hu0/7/0/34/1 1c0002a8   2   0    0    27  40080      0    etm_local    100G      0
Hu0/7/0/34/1 1c0002a8   2   0    0    27  33712      0   etm_global    100G      0
FH0/7/0/35   1c0002b0   2   0    0    28  40088      0    etm_local    400G      0
FH0/7/0/35   1c0002b0   2   0    0    28  33720      0   etm_global    400G      0
FH0/7/0/1    1c0002b8   0   2    0    29  40072      0    etm_local    400G      4
FH0/7/0/1    1c0002b8   0   2    0    29  33352      0   etm_global    400G      4
FH0/7/0/2    1c0002c0   0   2    1    30  52824      0    etm_local    400G      5
FH0/7/0/2    1c0002c0   0   2    1    30  33368      0   etm_global    400G      5
FH0/7/0/3    1c0002c8   0   1    1    31  52808      0    etm_local    400G      3
FH0/7/0/3    1c0002c8   0   1    1    31  33384      0   etm_global    400G      3
FH0/7/0/5    1c0002d0   0   1    0    32  40088      0    etm_local    400G      2
FH0/7/0/5    1c0002d0   0   1    0    32  33400      0   etm_global    400G      2
FH0/7/0/7    1c0002d8   0   0    1    33  52808      0    etm_local    400G      1
FH0/7/0/7    1c0002d8   0   0    1    33  33416      0   etm_global    400G      1
FH0/7/0/11   1c0002f0   1   2    0    36  40088      0    etm_local    400G      4
FH0/7/0/11   1c0002f0   1   2    0    36  33464      0   etm_global    400G      4
FH0/7/0/13   1c0002f8   1   1    1    37  52808      0    etm_local    400G      3
FH0/7/0/13   1c0002f8   1   1    1    37  33480      0   etm_global    400G      3
FH0/7/0/14   1c000300   1   0    0    38  40088      0    etm_local    400G      0
FH0/7/0/14   1c000300   1   0    0    38  33528      0   etm_global    400G      0
FH0/7/0/15   1c000308   1   1    0    39  40088      0    etm_local    400G      2
FH0/7/0/15   1c000308   1   1    0    39  33496      0   etm_global    400G      2
FH0/7/0/17   1c000310   1   0    1    40  52808      0    etm_local    400G      1
FH0/7/0/17   1c000310   1   0    1    40  33512      0   etm_global    400G      1
FH0/7/0/19   1c000318   2   2    1    41  52808      0    etm_local    400G      5
FH0/7/0/19   1c000318   2   2    1    41  33544      0   etm_global    400G      5
FH0/7/0/20   1c000320   3   2    1    42  52824      0    etm_local    400G      5
FH0/7/0/20   1c000320   3   2    1    42  33592      0   etm_global    400G      5
FH0/7/0/21   1c000328   2   2    0    43  40088      0    etm_local    400G      4
FH0/7/0/21   1c000328   2   2    0    43  33560      0   etm_global    400G      4
FH0/7/0/23   1c000330   3   2    0    44  40072      0    etm_local    400G      4
FH0/7/0/23   1c000330   3   2    0    44  33576      0   etm_global    400G      4
FH0/7/0/25   1c000338   3   1    1    45  52808      0    etm_local    400G      3
FH0/7/0/25   1c000338   3   1    1    45  33608      0   etm_global    400G      3
FH0/7/0/26   1c000340   3   1    0    46  40088      0    etm_local    400G      2
FH0/7/0/26   1c000340   3   1    0    46  33624      0   etm_global    400G      2
FH0/7/0/27   1c000348   3   0    1    47  52808      0    etm_local    400G      1
FH0/7/0/27   1c000348   3   0    1    47  33640      0   etm_global    400G      1
FH0/7/0/29   1c000350   3   0    0   117  40088      0    etm_local    400G      0
FH0/7/0/29   1c000350   3   0    0   117  33656      0   etm_global    400G      0
FH0/7/0/31   1c000358   2   1    1   118  52808      0    etm_local    400G      3
FH0/7/0/31   1c000358   2   1    1   118  33672      0   etm_global    400G      3
FH0/7/0/32   1c000360   2   1    0   119  40088      0    etm_local    400G      2
FH0/7/0/32   1c000360   2   1    0   119  33688      0   etm_global    400G      2
FH0/7/0/33   1c000368   2   0    1   120  52808      0    etm_local    400G      1
FH0/7/0/33   1c000368   2   0    1   120  33704      0   etm_global    400G      1
Hu0/7/0/6    1c000378   0   0    1   121  52800      0    etm_local    100G      1
Hu0/7/0/6    1c000378   0   0    1   121  33408      0   etm_global    100G      1
Hu0/7/0/10   1c000410   1   2    1   122  52800      0    etm_local    100G      5
Hu0/7/0/10   1c000410   1   2    1   122  33440      0   etm_global    100G      5
Hu0/7/0/8    1c0007a8   1   2    1    34  52808      0    etm_local    100G      5
Hu0/7/0/8    1c0007a8   1   2    1    34  33448      0   etm_global    100G      5
Hu0/7/0/9    1c0007b8   0   0    0    35  40080      0    etm_local    100G      0
Hu0/7/0/9    1c0007b8   0   0    0    35  33424      0   etm_global    100G      0  
</code>
</pre>
</div>  

- NPU #: NPU number  
-- NPU number should be 0 for 8200/8700 (8000 fixed & centralized systems)      
-- NPU number should be should be 0, 1 for 88-LC1-52Y8H-EM line card (not shown above)  
-- NPU number should be should be 0, 1, 2, 3 for 88-LC1-36EH/88-LC1-12TH24FH-E line card  

- NPU core: Slice number  
-- Slice number should be between 0 and 5 for 8212-48FH-M (8000 fixed systems)  
-- Slice number should be between 0 and 3 for 8711-32FH-M (8000 fixed systems)  
-- Slice number shoulbe be 0,1, 2 for 8800 Line cards     
- PP port: IFG number, should be 0 or 1 within slice  
{: .notice}  


## Port allocation  

### Cisco 8212-48FH-M  
Cisco 8212-48FH-M is made of a single P100 NPU. This system has 24 ports of QSFP-DD800 and 24 ports of QSFP56-DD. It supports MACsec on all ports. All ports support PTP timing at Class C performance.    

![Slugger-photo.png]({{site.baseurl}}/images/Slugger-photo.png){: .full}    

![8212-48FH-M-2.png]({{site.baseurl}}/images/8212-48FH-M-2.png){: .full}  

| Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|------------|---------------|------------|---------------|------------|---------------|
| FH0/0/0/0  | 0/5/1         | FH0/0/0/16 | 0/4/1         | FH0/0/0/32 | 0/3/1         |
| FH0/0/0/1  | 0/0/1         | FH0/0/0/17 | 0/1/1         | FH0/0/0/33 | 0/3/1         |
| FH0/0/0/2  | 0/5/1         | FH0/0/0/18 | 0/4/1         | FH0/0/0/34 | 0/3/1         |
| FH0/0/0/3  | 0/0/1         | FH0/0/0/19 | 0/1/1         | FH0/0/0/35 | 0/3/1         |
| FH0/0/0/4  | 0/5/1         | FH0/0/0/20 | 0/4/1         | FH0/0/0/36 | 0/3/0         |
| FH0/0/0/5  | 0/0/1         | FH0/0/0/21 | 0/1/1         | FH0/0/0/37 | 0/3/0         |
| FH0/0/0/6  | 0/5/1         | FH0/0/0/22 | 0/4/1         | FH0/0/0/38 | 0/3/0         |
| FH0/0/0/7  | 0/0/1         | FH0/0/0/23 | 0/1/1         | FH0/0/0/39 | 0/3/0         |
| FH0/0/0/8  | 0/4/0         | FH0/0/0/24 | 0/5/0         | FH0/0/0/40 | 0/2/1         |
| FH0/0/0/9  | 0/1/0         | FH0/0/0/25 | 0/0/0         | FH0/0/0/41 | 0/2/1         |
| FH0/0/0/10 | 0/4/0         | FH0/0/0/26 | 0/5/0         | FH0/0/0/42 | 0/2/1         |
| FH0/0/0/11 | 0/1/0         | FH0/0/0/27 | 0/0/0         | FH0/0/0/43 | 0/2/1         |
| FH0/0/0/12 | 0/4/0         | FH0/0/0/28 | 0/5/0         | FH0/0/0/44 | 0/2/0         |
| FH0/0/0/13 | 0/1/0         | FH0/0/0/29 | 0/0/0         | FH0/0/0/45 | 0/2/0         |
| FH0/0/0/14 | 0/4/0         | FH0/0/0/30 | 0/5/0         | FH0/0/0/46 | 0/2/0         |
| FH0/0/0/15 | 0/1/0         | FH0/0/0/31 | 0/0/0         | FH0/0/0/47 | 0/2/0         |  

### Cisco 8711-32FH-M
Cisco 8711-32FH-M is made of a single P100 NPU. This system has 16 ports of QSFP-DD800 and 16 ports of QSFP56-DD. It supports MACsec on all ports.    
Cisco 8700 products embody this cutting-edge innovation of the 8000 portfolio, offering advanced features engineered for seamless integration and scalability. All ports support PTP timing at Class C performance.    

![Saturn photo7.png]({{site.baseurl}}/images/Saturn photo7.png){: .full}  

![8711-32FH-M-2.png]({{site.baseurl}}/images/8711-32FH-M-2.png){: .full}  

| Interface | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG | Interface  | NPU/Slice/IFG |
|-----------|---------------|------------|---------------|------------|---------------|------------|---------------|
| FH0/0/0/0 | 0/3/1         | FH0/0/0/8  | 0/2/1         | FH0/0/0/16 | 0/1/1         | FH0/0/0/24 | 0/0/1         |
| FH0/0/0/1 | 0/3/1         | FH0/0/0/9  | 0/2/1         | FH0/0/0/17 | 0/1/1         | FH0/0/0/25 | 0/0/1         |
| FH0/0/0/2 | 0/3/1         | FH0/0/0/10 | 0/2/1         | FH0/0/0/18 | 0/1/1         | FH0/0/0/26 | 0/0/1         |
| FH0/0/0/3 | 0/3/1         | FH0/0/0/11 | 0/2/1         | FH0/0/0/19 | 0/1/1         | FH0/0/0/27 | 0/0/1         |
| FH0/0/0/4 | 0/3/0         | FH0/0/0/12 | 0/2/0         | FH0/0/0/20 | 0/1/0         | FH0/0/0/28 | 0/0/0         |
| FH0/0/0/5 | 0/3/0         | FH0/0/0/13 | 0/2/0         | FH0/0/0/21 | 0/1/0         | FH0/0/0/29 | 0/0/0         |
| FH0/0/0/6 | 0/3/0         | FH0/0/0/14 | 0/2/0         | FH0/0/0/22 | 0/1/0         | FH0/0/0/30 | 0/0/0         |
| FH0/0/0/7 | 0/3/0         | FH0/0/0/15 | 0/2/0         | FH0/0/0/23 | 0/1/0         | FH0/0/0/31 | 0/0/0         |    


### 88-LC1-36EH  
The Cisco 88-LC1-36EH P100 based transport optimized line card provides 36 ports of 800G (high dense 400 GbE), aggregating to a total bandwidth of 28.8 Tbps.There are four P100 NPUs on this line card. Allow Flexible combination of 800G/400G/200G/100G/40G/10G Optics and port based Breakout options. This line card doesn't support MACsec. All ports support PTP timing at Class C performance.    

![Fury photo.png]({{site.baseurl}}/images/Fury photo.png)  

![Fury2.png]({{site.baseurl}}/images/Fury2.png)  

| Interface                                | NPU/Slice/IFG | Interface                                | NPU/Slice/IFG |
|------------------------------------------|---------------|------------------------------------------|---------------|
| FH0/x/0/0/0, FH0/x/0/1/0, FH0/x/0/1/1    | 0/2/0         | FH0/x/0/18/0, FH0/x/0/19/0, FH0/x/0/19/1 | 2/2/1         |
| FH0/x/0/0/1,FH0/x/0/2/0, FH0/x/0/2/1     | 0/2/1         | FH0/x/0/18/1,FH0/x/0/21/0, FH0/x/0/21/1  | 2/2/0         |
| FH0/x/0/3/0, FH0/x/0/3/1, FH0/x/0/4/0    | 0/1/1         | FH0/x/0/20/0, FH0/x/0/20/1, FH0/x/0/22/1 | 3/2/1         |
| FH0/x/0/4/1, FH0/x/0/5/0, FH0/x/0/5/1    | 0/1/0         | FH0/x/0/22/0, FH0/x/0/23/0, FH0/x/0/23/1 | 3/2/0         |
| FH0/x/0/6/0, FH0/x/0/7/0, FH0/x/0/7/1    | 0/0/1         | FH0/x/0/24/0,FH0/x/0/25/0, FH0/x/0/25/1  | 3/1/1         |
| FH0/x/0/6/1, FH0/x/0/9/0, FH0/x/0/9/1    | 0/0/0         | FH0/x/0/24/1, FH0/x/0/26/0, FH0/x/0/26/1 | 3/1/0         |
| FH0/x/0/8/0, FH0/x/0/8/1, FH0/x/0/10/0   | 1/2/1         | FH0/x/0/27/0, FH0/x/0/27/1, FH0/x/0/28/0 | 3/0/1         |
| FH0/x/0/10/1, FH0/x/0/11/0, FH0/x/0/11/1 | 1/2/0         | FH0/x/0/28/1,FH0/x/0/29/0, FH0/x/0/29/1  | 3/0/0         |
| FH0/x/0/12/0, FH0/x/0/13/0, FH0/x/0/13/1 | 1/1/1         | FH0/x/0/30/0, FH0/x/0/31/0, FH0/x/0/31/1 | 2/1/1         |
| FH0/x/0/14/0, FH0/x/0/14/1, FH0/x/0/16/1 | 1/0/0         | FH0/x/0/30/1, FH0/x/0/32/0, FH0/x/0/32/1 | 2/1/0         |
| FH0/x/0/12/1,FH0/x/0/15/0, FH0/x/0/15/1  | 1/1/0         | FH0/x/0/33/0, FH0/x/0/33/1, FH0/x/0/34/0 | 2/0/1         |
| FH0/x/0/16/0, FH0/x/0/17/0, FH0/x/0/17/1 | 1/0/1         | FH0/x/0/34/1,FH0/x/0/35/0, FH0/x/0/35/1  | 2/0/0         |  

**Note**: x is slot number in Cisco 8800 router  

### 88-LC1-12TH24FH-E  
The Cisco 88-LC1-12TH24FH-E  P100 based collapsed core, peering, aggregation and WAN Internet Gateway optimized line card provides 24 ports of 400G, 12 ports of 200G, aggregating to a total bandwidth of 12 Tbps. There are four P100 NPUs on this line card.  Allow Flexible combination of 400G/100G/40G/25G/10G Optics and port based Breakout options. This line card doesn't support MACsec. All ports support PTP timing at Class C performance.    

![Screenshot 2024-09-20 at 11.50.51 AM.png]({{site.baseurl}}/images/Screenshot 2024-09-20 at 11.50.51 AM.png)  

![Everest-2.png]({{site.baseurl}}/images/Everest-2.png)  


| Interface                 | NPU/Slice/IFG | Interface                | NPU/Slice/IFG | Interface                | NPU/Slice/IFG |
|---------------------------|---------------|--------------------------|---------------|--------------------------|---------------|
| Hu0/x/0/6/1,<br>FH0/x/0/9    | 0/0/0         | FH0/x/0/15,<br>Hu0/x/0/12/1 | 1/1/0        | FH0/x/0/21,<br>Hu0/x/0/18/1 | 2/2/0        |
| FH0/x/0/7,<br>Hu0/x/0/6/0    | 0/0/1         | FH0/x/0/13,<br>Hu0/x/0/12/0 | 1/1/1        | FH0/x/0/19,<br>Hu0/x/0/18/0 | 2/2/1        |
| FH0/x/0/5,<br>Hu0/x/0/4/1    | 0/1/0         | FH0/x/0/11,<br>Hu0/x/0/10/1 | 1/2/0        | FH0/x/0/29,<br>Hu0/x/0/28/1 | 3/0/0         |
| FH0/x/0/3,<br>Hu0/x/0/4/0    | 0/1/1         | FH0/x/0/8,<br>Hu0/x/0/10/0  | 1/2/1        | FH0/x/0/27,<br>Hu0/x/0/28/0 | 3/0/1         |
| FH0/x/0/1,<br>Hu0/x/0/0/0    | 0/2/0         | FH0/x/0/35,<br>Hu0/x/0/34/1 | 2/0/0         | FH0/x/0/26,<br>Hu0/x/0/24/1 | 3/1/0        |
| FH0/x/0/2,<br>Hu0/x/0/0/1    | 0/2/1         | FH0/x/0/33,<br>Hu0/x/0/34/0 | 2/0/1         | FH0/x/0/25,<br>Hu0/x/0/24/0 | 3/1/1        |
| FH0/x/0/14,<br>Hu0/x/0/16/1  | 1/0/0         | FH0/x/0/32,<br>Hu0/x/0/30/1 | 2/1/0        | FH0/x/0/23,<br>Hu0/x/0/22/0 | 3/2/0        |
| FH0/x/0/17,<br>Hu0/x/0/16/0  | 1/0/1         | FH0/x/0/31,<br>Hu0/x/0/30/0 | 2/1/1        | FH0/x/0/20,<br>Hu0/x/0/22/1 | 3/2/1        |  

**Note**: x is slot number in Cisco 8800 router  

### 88-LC1-52Y8H-EM  
The Cisco 88-LC1-52Y8H-EM P100 based collapsed core, peering, aggregation and WAN Internet Gateway optimized line card provides 4 ports of 400G, 8 ports of 100G, and 52 ports of 10G/25G, aggregating to a total bandwidth of 3.7 Tbps. There are two P100 NPUs on this line card. Allow Flexible combination of 400G/100G/40G/25G/10G/1G Optics and port based Breakout options. This line card supports MACsec on port speed at line rate. All ports support PTP timing at Class C performance.    

![K2-photo5.png]({{site.baseurl}}/images/K2-photo5.png)  

![K2-2.png]({{site.baseurl}}/images/K2-2.png)  

| Interface                                                                                        | NPU/Slice/IFG | Interface                                                                                                           | NPU/Slice/IFG | Interface                                                                                                              | NPU/Slice/IFG | Interface                                                                                      | NPU/Slice/IFG |
|--------------------------------------------------------------------------------------------------|---------------|---------------------------------------------------------------------------------------------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------|---------------|------------------------------------------------------------------------------------------------|---------------|
| FH0/x/0/31                                                                                       | 0/0/0         | TF0/x/0/7<br>TF0/x/0/8<br>TF0/x/0/10<br>TF0/x/0/11<br>TF0/x/0/13<br>TF0/x/0/14<br>TF0/x/0/16<br>TF0/x/0/17                        | 0/1/1         | TF0/x/0/61<br>TF0/x/0/58<br>TF0/x/0/55<br>TF0/x/0/52<br>TF0/x/0/49<br>TF0/x/0/34<br>TF0/x/0/37<br>TF0/x/0/40<br>TF0/x/0/43<br>TF0/x/0/46 | 1/0/0         | TF0/x/0/35<br>TF0/x/0/36<br>TF0/x/0/38<br>TF0/x/0/39<br>TF0/x/0/41<br>TF0/x/0/42<br>TF0/x/0/44<br>TF0/x/0/45 | 1/1/1        |
| FH0/x/0/30                                                                                       | 0/0/1         | Hu0/x/0/5<br>Hu0/x/0/4<br>Hu0/x/0/2<br>Hu0/x/0/1                                                                          | 0/2/0         | Hu0/x/0/63<br>Hu0/x/0/59<br>Hu0/x/0/60<br>Hu0/x/0/62                                                                         | 1/0/1         | FH0/x/0/33                                                                                     | 1/2/0        |
| TF0/x/0/19<br>TF0/x/0/20<br>TF0/x/0/22<br>TF0/x/0/23<br>TF0/x/0/25<br>TF0/x/0/26<br>TF0/x/0/28<br>TF0/x/0/29   | 0/1/0         | TF0/x/0/0<br>TF0/x/0/3<br>TF0/x/0/6<br>TF0/x/0/9<br>TF0/x/0/12<br>TF0/x/0/15<br>TF0/x/0/18<br>TF0/x/0/21<br>TF0/x/0/24<br>TF0/x/0/27  | 0/2/1         | TF0/x/0/57<br>TF0/x/0/56<br>TF0/x/0/54<br>TF0/x/0/53<br>TF0/x/0/51<br>TF0/x/0/50<br>TF0/x/0/48<br>TF0/x/0/47                         | 1/1/0        | FH0/x/0/32                                                                                     | 1/2/1        |  

**Note**: x is slot number in Cisco 8800 router  


Also you can find the first part of this post here: [Cisco 8000 Port assignment (Part 1)](https://xrdocs.io/8000/tutorials/cisco-8000-port-assignment/)

## Modification History  

| Version | Data     | Author(s)     | Comments            |
|---------|----------|---------------|---------------------|
| 1       | September-26 | Chang Soo Lee | Initial Publication for P100 based platforms |
