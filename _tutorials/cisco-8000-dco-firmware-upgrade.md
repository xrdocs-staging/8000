---
published: true
date: '2024-07-02 11:31 +0200'
title: Cisco 8000 Optics Firmware Management
author: Frederic Cuiller
tags:
  - Cisco 8000
  - firmware
  - DCO
  - Bright ZR+
position: top
excerpt: >-
  This article aims at documenting QSFP-DD optical modules firmware upgrade on
  Cisco 8000 routers.
---
{% include toc icon="table" title="Cisco 8000 Optics Firmware Management" %}

![8000-dco-upgrade.png]({{site.baseurl}}/images/8000-dco-upgrade.png)

# Introduction

This Method of Procedure (MOP) aims at documenting QSFP-DD optical modules firmware upgrade on Cisco 8000 routers.  
Like any Cisco 8000 systems’ components such route processor, line card, or even power supplies, optics also run embedded software. This software is used to configure, control and monitor the module. For Digital Coherent Optics (DCO), the Common Management Interface Specification (CMIS) interface is responsible for communication between the host (Cisco 8000 router) and the managed module (e.g Bright ZR+ DP04QSDD-HE0).  
While optics usually ship with a specific & stable firmware version from manufacturing, there are cases where it’s required to upgrade them. Such conditions include new functionality, software defect or interoperability fix.

# Initial State

In this example, we’ll upgrade a [Cisco 400G QSFP-DD Bright ZR+ module](https://www.cisco.com/c/en/us/products/collateral/interfaces-modules/transceiver-modules/400g-qsfp-dd-high-power-optical-module-ds.html) from 70.120.14 to 70.130.21. The procedure described here is also applicable to different modules and 3rd party vendors.  

The following command helps to identify which firmware a module is currently running:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#sh controllers Optics0/0/0/1
Thu Jun 27 22:36:14.025 JST
 Controller State: Up
 Transport Admin State: In Service
 Laser State: On
 Host Squelch Status: Enable
 LED State: Green
 FEC State: FEC ENABLED
 Power Mode: High
 Dom Data Status: Ready

 Optics Status

         Optics Type:  QDD 400G BRT ZRP
         DWDM carrier Info: C BAND, MSA ITU Channel=67, Frequency=192.80THz,
         Wavelength=1554.940nm
         Loopback Host : None
         Loopback Media : None

         Alarm Status:
         -------------
         Detected Alarms: None


         LOS/LOL/Fault Status:

         Alarm Statistics:

         -------------
         HIGH-RX-PWR = 0            LOW-RX-PWR = 0
         HIGH-TX-PWR = 0            LOW-TX-PWR = 0
         HIGH-LBC = 0               HIGH-DGD = 0
         OOR-CD = 0                 OSNR = 1
         WVL-OOL = 0                MEA  = 0
         IMPROPER-REM = 0
         TX-POWER-PROV-MISMATCH = 0
         Laser Bias Current = 234.3 mA
         Actual TX Power = 0.00 dBm
         Actual TX Power(mW) = 1.00 mW
         RX Power = -2.84 dBm
         RX Power(mW) = 0.51 mW
         RX Signal Power = -3.16 dBm
         Frequency Offset = -39 MHz
         Laser Temperature = 56.64 Celsius
         Laser Age = 0 %
         DAC Rate = 1x1.25

         Performance Monitoring: Enable

         THRESHOLD VALUES
         ----------------

         Parameter                 High Alarm  Low Alarm  High Warning  Low Warning
         ------------------------  ----------  ---------  ------------  -----------
         Rx Power Threshold(dBm)         13.0      -24.0          10.0        -21.0
         Rx Power Threshold(mW)          19.9        0.0          10.0          0.0
         Tx Power Threshold(dBm)          6.0      -18.0           4.0        -16.0
         Tx Power Threshold(mW)           3.9        0.0           2.5          0.0
         LBC Threshold(mA)               0.00       0.00          0.00         0.00
         Temp. Threshold(celsius)       83.00      -5.00         78.00        15.00
         Voltage Threshold(volt)         3.46       3.13          3.43         3.16

         LBC High Threshold = 98 %
         Configured Tx Power = 0.00 dBm
         Configured Tx Power(mW) = 1.00 mW
         Configured CD High Threshold = 160000 ps/nm
         Configured CD lower Threshold = -160000 ps/nm
         Configured OSNR lower Threshold = 9.00 dB
         Configured DGD Higher Threshold = 80.00 ps
         Baud Rate =  60.1385459900 GBd
         Modulation Type: 16QAM
         Chromatic Dispersion 0 ps/nm
         Configured CD-MIN -13000 ps/nm  CD-MAX 13000 ps/nm
         Second Order Polarization Mode Dispersion = 42.00 ps^2
         Optical Signal to Noise Ratio = 35.40 dB
         SNR = 19.40 dB
         Polarization Dependent Loss = 2.50 dB
         Polarization Change Rate = 0.00 rad/s
         Differential Group Delay = 4.00 ps

         Temperature = 63.00 Celsius
         Voltage = 3.29 V

 Transceiver Vendor Details

         Form Factor            : QSFP-DD
         Optics type            : QDD 400G BRT ZRP
         Name                   : CISCO-ACACIA
         OUI Number             : 7c.ffffffb2.5c
         Part Number            : DP04QSDD-HE0-190
         Rev Number             : A
         Serial Number          : ACA2740002C
         PID                    : DP04QSDD-HE0
         VID                    : V01
         <mark>Firmware Version       : Major.Minor.Build</mark>
                 <mark>Active         : 70.120.14</mark>
                 <mark>Inactive       : 70.120.14</mark>
         Date Code(yy/mm/dd)    : 23/09/14
         Fiber Connector Type: LC
         Otn Application Code: Undefined
         Sonet Application Code: Undefined
</code>
</pre>
</div>

Similar information can be retrieved using Cisco-IOS-XR-controller-optics-oper:optics-oper/optics-ports/optics-port/optics-info YANG path with transceiver-info/vendor-info leaf.

# Prerequisistes

The very first step is to get the new firmware. Contact your Cisco representative or Cisco TAC to get the new firmware.

Every firmware is provided with its own MD5 signature to verify file integrity:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
eng 1929216 May 15 09:11 gl2qsdd_gl2sfp8_70_130_21.ackit     --- firmware file
eng      66 May 15 09:12 gl2qsdd_gl2sfp8_70_130_21.ackit.md5 --- firmware signature

[06:49:42 fcuiller@server /QDD_400G_BRIGHT/firmware]$ cat gl2qsdd_gl2sfp8_70_130_21.ackit.md5
<mark>614a66be053114134fc92e3be301a02a  gl2qsdd_gl2sfp8_70_130_21.ackit</mark>


[06:49:42 fcuiller@server /QDD_400G_BRIGHT/firmware]$ <mark>md5sum gl2qsdd_gl2sfp8_70_130_21.ackit</mark>
<mark>614a66be053114134fc92e3be301a02a</mark>  gl2qsdd_gl2sfp8_70_130_21.ackit
</code>
</pre>
</div>

Firmware must be copied to the Cisco 8000 router harddisk:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
[deaccess@asj ~]$ scp gl2qsdd_gl2sfp8_70_130_21.ackit cisco@10.71.242.15:/harddisk:/
Password:
gl2qsdd_gl2sfp8_70_130_21.ackit                                                                                                                                                                                                       100% 1884KB   1.8MB/s   00:00
[deaccess@asj ~]$
</code>
</pre>
</div>

File integrity is verified one more time from the router to make sure it’s not been corrupted in transit:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#sh md5 file /harddisk:/gl2qsdd_gl2sfp8_70_130_21.ackit
Thu Jun 27 22:55:59.491 JST
<mark>614a66be053114134fc92e3be301a02a</mark>
RP/0/RP0/CPU0:CISCO-8200#
</code>
</pre>
</div>


# Pre-upgrade Tasks

It’s recommended to perform a sanity check before performing the firmware upgrade. While the list of KPI is customer dependent, the following information must be captured on both link ends:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
sh controllers optics 0/0/0/1
sh controllers optics 0/0/0/1 pm
sh controllers  coherentDSP 0/0/0/1
sh controllers fourhundredGigE0/0/0/1 all
sh interfaces fourHundredGigE 0/0/0/1
</code>
</pre>
</div>

In addition to this list, Phil Bedard published a list of KPI to monitor in [this article]({{site.url}}/design/blogs/xr-dco-monitoring).

Following CLI will provide a quick consolidated view:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#sh controllers optics 0/0/0/1 summary
Thu Jun 27 23:02:54.260 JST

Port                Controller State        Admin State            LED State         Lane   Laser Bias   TX Power     RX Power     Wavelength     Optics Type       Description
                                                                                                          (dBm)        (dBm)        (nm)
------------------  ----------------------  ---------------------  ----------------  -----  ------------ -----------  ----------   -----------    --------------    -----------
Optics 0/0/0/1      Up                      In Service             Green             1      246.2mA      0.00         -2.60        1554.940       QSFP-DD-400G-BRIGHT-ZRP
RP/0/RP0/CPU0:CISCO-8200#
</code>
</pre>
</div>

The purpose of this sanity check is to capture current optic state and compare it after the firmware upgrade is complete.  

Once the sanity check is done, it’s recommended to drain/isolate the link. This can be achieved using regular routing techniques (ISIS overload bit, OSPF max metric, MPLS traffic engineering, BGP attributes, etc.).  

Optic module can be then administratively shutdown. This brings down associated ethernet ports and routing protocols:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200(config)#controller optics 0/0/0/1
RP/0/RP0/CPU0:CISCO-8200(config-Optics)#   shutdown
RP/0/RP0/CPU0:CISCO-8200(config-Optics)#commit
Thu Jun 27 23:14:49.640 JST
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.678 JST: ifmgr[337]: %PKT_INFRA-LINK-5-CHANGED : <mark>Interface Optics0/0/0/1, changed state to Administratively Down</mark>
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.708 JST: config[66279]: %MGBL-CONFIG-6-DB_COMMIT : Configuration committed by user 'cisco'. Use 'show configuration commit changes 1000000261' to view the changes.
RP/0/RP0/CPU0:CISCO-8200(config-Optics)#RP/0/RP0/CPU0:2024 Jun 27 23:14:49.825 JST: npu_drvr[265]: %PLATFORM-VETH_PD-2-RX_FAULT : Interface FourHundredGigE0/0/0/1, Detected Local Fault
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.833 JST: bfd_agent[376]: %L2-BFD-6-SESSION_STATE_DOWN : BFD session to neighbor fe80::2 on interface FourHundredGigE0/0/0/1 has gone down. Reason: Control timer expired
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.836 JST: BM-DISTRIB[1188]: %L2-BM-5-MBR_BFD_SESSION_DOWN : The BFD session on link FourHundredGigE0/0/0/1 in Bundle-Ether1001 is down due to the receipt of a Down SCN. The member will be removed from the active members of the bundle.
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.836 JST: BM-DISTRIB[1188]: %L2-BM-6-ACTIVE : FourHundredGigE0/0/0/1 is no longer Active as part of Bundle-Ether1001 (BFD state of this link is Down)
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.836 JST: isis[1005]: %ROUTING-ISIS-5-ADJCHANGE : ISIS (100): Adjacency to CISCO-8200 (Bundle-Ether1001) (L2) Down, Interface state down
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.836 JST: ifmgr[337]: %PKT_INFRA-LINK-3-UPDOWN : Interface Bundle-Ether1001, changed state to Down
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.836 JST: ifmgr[337]: %PKT_INFRA-LINEPROTO-5-UPDOWN : Line protocol on Interface Bundle-Ether1001, changed state to Down
RP/0/RP0/CPU0:2024 Jun 27 23:14:49.839 JST: bfd[1189]: %L2-BFD-6-SESSION_STATE_DOWN : BFD session to neighbor fe80::2 on interface Bundle-Ether1001 has gone down. Reason: Control timer expired
RP/0/RP0/CPU0:2024 Jun 27 23:14:50.026 JST: ifmgr[337]: %PKT_INFRA-LINK-3-UPDOWN : <mark>Interface FourHundredGigE0/0/0/1, changed state to Down</mark>
RP/0/RP0/CPU0:2024 Jun 27 23:14:50.026 JST: ifmgr[337]: %PKT_INFRA-LINEPROTO-5-UPDOWN : Line protocol on Interface FourHundredGigE0/0/0/1, changed state to Down
RP/0/RP0/CPU0:2024 Jun 27 23:14:50.028 JST: BM-DISTRIB[1188]: %L2-BM-5-MBR_BFD_NOT_RUNNING : The BFD session on link FourHundredGigE0/0/0/1 in Bundle-Ether1001 is no longer required.
</code>
</pre>
</div>

# Firmware Upgrade

In Cisco 8000, optics firmwares have been decoupled from the Network Operating System (NOS) which is IOS XR. Benefits of decoupling optics firmwares from the NOS include faster firmware delivery, 3rd party optics support (including firmware management) and no IOS XR SMU/FPD overhead. This means the Field Programmable Device (FPD) process is not used here. Instead, a new ‘upgrade optics port’ command is used:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#<mark>upgrade optics port ?</mark>
  WORD  Port number
        List of comma separated port numbers
        Port range, first-last
        all: All ports
</code>
</pre>
</div>

The command can upgrade one specific port, a group of ports or all ports from a system or a given line card.

**Note:** if a range of ports or ‘all’ keyword is used, ports will be upgraded sequencially (one by one)
{: .notice--info}

In this case Optics0/0/0/1 firmware is upgraded. The MD5 hash is provided as an argument to verify file integrity after the transfer. The filename source is the absolute location of the firmware file. A syslog indicates the beginning of the procedure:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#<mark>upgrade optics port 1 filename /harddisk:/gl2qsdd_gl2sfp8_70_130_21.ackit md5sum 614a66be053114134fc92e3be301a02a location 0/rp0/CPU0</mark>
Thu Jun 27 23:55:20.816 JST

Ctrl: 0 Port 1
==================
FW Upgrade Trigger Success

RP/0/RP0/CPU0:CISCO-8200#RP/0/RP0/CPU0:2024 Jun 27 23:55:50.895 JST: optics_driver[283]: %L2-OPTICS-6-QDD_FW_UPGRADE_STARTED : <mark>QDD Firmware upgrade started on Optics0/0/0/1</mark>
</code>
</pre>
</div>

The upgrade process can be monitored with following CLI:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#show optics firmware upgrade port 1 location 0/RP0/CPU0
Thu Jun 27 23:55:56.975 JST
Ctrl: 0 Port 1
==================
download::[1], commit::[0]

RP/0/RP0/CPU0:CISCO-8200#show optics firmware upgrade port 1 location 0/RP0/CPU0
Fri Jun 28 00:14:26.354 JST
Ctrl: 0 Port 1
==================
download::[90], commit::[0]
</code>
</pre>
</div>

Firmware is first downloaded from the router to the module through I2C interface and then applied/commited:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#show optics firmware upgrade port 1 location 0/RP0/CPU0
Fri Jun 28 00:16:52.818 JST
Ctrl: 0 Port 1
==================
download::[100], commit::[16]
</code>
</pre>
</div>

Once the upgrade is succesfull, following syslog appears:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#RP/0/RP0/CPU0:2024 Jun 28 00:17:12.035 JST: optics_driver[283]: %L2-OPTICS-2-QDD_FW_UPGRADE_DONE : <mark>QDD Firmware upgrade completed successfully on Optics0/0/0/1</mark>
</code>
</pre>
</div>

The process takes ~20 min per module (tested on IOS XR 24.2.1).

The new firmware version can be verified:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200#sh controllers optics 0/0/0/1
Fri Jun 28 00:18:27.148 JST
 Controller State: Administratively Down
 Transport Admin State: Out Of Service
 Laser State: Off
 Host Squelch Status: Enable
 LED State: Off
 FEC State: FEC ENABLED
 Power Mode: Low
 Dom Data Status: Ready

 Optics Status

         Optics Type:  QDD 400G BRT ZRP
         DWDM carrier Info: C BAND, MSA ITU Channel=67, Frequency=192.80THz,
         Wavelength=1554.940nm
         Loopback Host : None
         Loopback Media : None

         Alarm Status:
         -------------
         Detected Alarms: None


         LOS/LOL/Fault Status:

         Alarm Statistics:

         -------------
         HIGH-RX-PWR = 0            LOW-RX-PWR = 0
         HIGH-TX-PWR = 0            LOW-TX-PWR = 0
         HIGH-LBC = 0               HIGH-DGD = 0
         OOR-CD = 0                 OSNR = 2
         WVL-OOL = 0                MEA  = 0
         IMPROPER-REM = 0
         TX-POWER-PROV-MISMATCH = 0
         Laser Bias Current = 0.0 mA
         Actual TX Power = -40.00 dBm
         Actual TX Power(mW) = 0.00 mW
         RX Power = -40.00 dBm
         RX Power(mW) = 0.00 mW
         RX Signal Power = -40.00 dBm
         Frequency Offset = 0 MHz
         Laser Temperature = 0.00 Celsius
         Laser Age = 0 %
         DAC Rate = 1x1.25

         Performance Monitoring: Enable

         THRESHOLD VALUES
         ----------------

         Parameter                 High Alarm  Low Alarm  High Warning  Low Warning
         ------------------------  ----------  ---------  ------------  -----------
         Rx Power Threshold(dBm)         13.0      -24.0          10.0        -21.0
         Rx Power Threshold(mW)          19.9        0.0          10.0          0.0
         Tx Power Threshold(dBm)          6.0      -18.0           4.0        -16.0
         Tx Power Threshold(mW)           3.9        0.0           2.5          0.0
         LBC Threshold(mA)               0.00       0.00          0.00         0.00
         Temp. Threshold(celsius)       83.00      -5.00         78.00        15.00
         Voltage Threshold(volt)         3.46       3.13          3.43         3.16

         LBC High Threshold = 98 %
         Configured Tx Power = 0.00 dBm
         Configured Tx Power(mW) = 1.00 mW
         Configured CD High Threshold = 160000 ps/nm
         Configured CD lower Threshold = -160000 ps/nm
         Configured OSNR lower Threshold = 9.00 dB
         Configured DGD Higher Threshold = 80.00 ps
         Baud Rate =  60.1385459900 GBd
         Modulation Type: 16QAM
         Chromatic Dispersion 0 ps/nm
         Configured CD-MIN -13000 ps/nm  CD-MAX 13000 ps/nm
         Second Order Polarization Mode Dispersion = 0.00 ps^2
         Optical Signal to Noise Ratio = 0.00 dB
         SNR = 0.00 dB
         Polarization Dependent Loss = 0.00 dB
         Polarization Change Rate = 0.00 rad/s
         Differential Group Delay = 0.00 ps

         Temperature = 32.00 Celsius
         Voltage = 3.35 V

 Transceiver Vendor Details

         Form Factor            : QSFP-DD
         Optics type            : QDD 400G BRT ZRP
         Name                   : CISCO-ACACIA
         OUI Number             : 7c.ffffffb2.5c
         Part Number            : DP04QSDD-HE0-190
         Rev Number             : A
         Serial Number          : ACA2740002Y
         PID                    : DP04QSDD-HE0
         VID                    : V01
         <mark>Firmware Version       : Major.Minor.Build</mark>
                 <mark>Active         : 70.130.21</mark>
                 Inactive       : 70.120.14
         Date Code(yy/mm/dd)    : 23/09/16
         Fiber Connector Type: LC
         Otn Application Code: Undefined
         Sonet Application Code: Undefined
</code>
</pre>
</div>

# Post-upgrade Tasks

Once the upgrade is succesfull, the optics controller can be restored. The link will be brought up and the final sanity check can be performed.

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:CISCO-8200(config)#controller optics 0/0/0/1
RP/0/RP0/CPU0:CISCO-8200(config-Optics)#<mark>no shutdown</mark>
RP/0/RP0/CPU0:CISCO-8200(config-Optics)#commit
Fri Jun 28 00:21:26.266 JST
RP/0/RP0/CPU0:2024 Jun 28 00:21:26.304 JST: ifmgr[337]: %PKT_INFRA-LINK-5-CHANGED : Interface Optics0/0/0/1, changed state to Down
RP/0/RP0/CPU0:2024 Jun 28 00:21:26.335 JST: config[66220]: %MGBL-CONFIG-6-DB_COMMIT : Configuration committed by user 'cisco'. Use 'show configuration commit changes 1000000262' to view the changes.
RP/0/RP0/CPU0:CISCO-8200(config-Optics)#RP/0/RP0/CPU0:2024 Jun 28 00:21:28.408 JST: ifmgr[337]: %PKT_INFRA-LINK-5-CHANGED : Interface Optics0/0/0/1, changed state to Up
RP/0/RP0/CPU0:2024 Jun 28 00:21:29.795 JST: optics_driver[283]: %PKT_INFRA-FM-4-FAULT_MINOR : ALARM_MINOR :OSNR :DECLARE :Optics0/0/0/1:
RP/0/RP0/CPU0:2024 Jun 28 00:22:57.795 JST: optics_driver[283]: %PKT_INFRA-FM-4-FAULT_MINOR : ALARM_MINOR :OPTICS HOST PCS TX REMOTE FAULT ALARM LANE0 :DECLARE :0/RP0/CPU0:  Optics0/0/0/1
RP/0/RP0/CPU0:2024 Jun 28 00:22:57.795 JST: optics_driver[283]: %PKT_INFRA-FM-4-FAULT_MINOR : ALARM_MINOR :OPTICS HOST PCS RX REMOTE FAULT ALARM LANE0 :DECLARE :0/RP0/CPU0:  Optics0/0/0/1
RP/0/RP0/CPU0:2024 Jun 28 00:23:08.298 JST: optics_driver[283]: %PKT_INFRA-FM-4-FAULT_MINOR : ALARM_MINOR :OPTICS HOST PCS RX REMOTE FAULT ALARM LANE0 :CLEAR :0/RP0/CPU0:  Optics0/0/0/1
RP/0/RP0/CPU0:2024 Jun 28 00:23:09.295 JST: optics_driver[283]: %PKT_INFRA-FM-4-FAULT_MINOR : ALARM_MINOR :OPTICS HOST PCS TX REMOTE FAULT ALARM LANE0 :CLEAR :0/RP0/CPU0:  Optics0/0/0/1
RP/0/RP0/CPU0:2024 Jun 28 00:23:11.295 JST: optics_driver[283]: %PKT_INFRA-FM-4-FAULT_MINOR : ALARM_MINOR :OSNR :CLEAR :Optics0/0/0/1:

RP/0/RP0/CPU0:CISCO-8200(config-Optics)#RP/0/RP0/CPU0:2024 Jun 28 00:24:58.110 JST: ifmgr[337]: %PKT_INFRA-LINK-3-UPDOWN : Interface FourHundredGigE0/0/0/1, changed state to Up
RP/0/RP0/CPU0:2024 Jun 28 00:24:58.111 JST: ifmgr[337]: %PKT_INFRA-LINEPROTO-5-UPDOWN : Line protocol on Interface FourHundredGigE0/0/0/1, changed state to Up
RP/0/RP0/CPU0:2024 Jun 28 00:25:00.128 JST: bfd_agent[376]: %L2-BFD-6-SESSION_DAMPENING_ON : Session to neighbor fe80::2 on interface FourHundredGigE0/0/0/1 entered Dampened state (initial: 16000 ms,secondary: 20000 ms,maximum: 600000 ms).
RP/0/RP0/CPU0:2024 Jun 28 00:25:00.130 JST: BM-DISTRIB[1188]: %L2-BM-6-MBR_BFD_STARTING : The BFD session on link FourHundredGigE0/0/0/1 in Bundle-Ether1001 is starting. Waiting indefinitely for peer to establish session.
RP/0/RP0/CPU0:2024 Jun 28 00:25:15.187 JST: bfd_agent[376]: %L2-BFD-6-SESSION_DAMPENING_OFF : Session to neighbor fe80::2 on interface FourHundredGigE0/0/0/1 moved out of Dampened state.
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.290 JST: bfd_agent[376]: %L2-BFD-6-SESSION_STATE_UP : BFD session to neighbor fe80::2 on interface FourHundredGigE0/0/0/1 is up
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.292 JST: BM-DISTRIB[1188]: %L2-BM-6-MBR_BFD_SESSION_UP : The BFD session on link FourHundredGigE0/0/0/1 in Bundle-Ether1001 has gone UP.
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.292 JST: BM-DISTRIB[1188]: %L2-BM-6-ACTIVE : FourHundredGigE0/0/0/1 is Active as part of Bundle-Ether1001
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.292 JST: ifmgr[337]: %PKT_INFRA-LINK-3-UPDOWN : Interface Bundle-Ether1001, changed state to Up
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.295 JST: bfd[1189]: %L2-BFD-6-SESSION_STATE_UP : BFD session to neighbor fe80::2 on interface Bundle-Ether1001 is up
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.295 JST: ifmgr[337]: %PKT_INFRA-LINEPROTO-5-UPDOWN : Line protocol on Interface Bundle-Ether1001, changed state to Up
RP/0/RP0/CPU0:2024 Jun 28 00:25:16.317 JST: isis[1005]: %ROUTING-ISIS-5-ADJCHANGE : ISIS (100): Adjacency to CISCO-8200 (Bundle-Ether1001) (L2) Up, New adjacency
</code>
</pre>
</div>

Once KPIs are confirmed to be compliant, traffic can be finally restored.

# Conclusion

This tutorial demonstrated how to upgrade an Acacia Bright 400ZR+ QSFP-DD Coherent Pluggable Module on a Cisco 8000 router. The same procedure can be used for other modules (ZR, ZR+) and vendors (e.g Marvell).

# Acknowledgement
I’d like to thanks Phil Bedard, Sreenivasa Rao Bandlamudi, Takumi Takiguchi & Marco Liri.
