---
published: true
date: '2024-09-02 15:48 +0200'
title: Cisco 8000 Disk Encryption
author: Frederic Cuiller
excerpt: >-
  This article aims to describe the disk encryption feature on Cisco 8000
  routers.  This functionality creates an encrypted partition where sensitive
  data such configuration, certificates and keys are stored. 
tags:
  - Cisco 8000
  - Disk Encryption
  - dm-crypt
  - TAm
position: top
---
{% include toc icon="table" title="Cisco 8000 Disk Encryption" %}

**Note:** Disk encryption is now enabled by default on all Cisco 8000 routers since IOS-XR 25.2.1, without possibility to disable it. While the technical content of the article below remains valid, the procedure described only applies for pre IOS-XR 25.2.1 releases. Starting IOS-XR 25.2.1 and onward, there is no more configuration needed to enable disk encryption.
{: .notice--info}

# Introduction
This article aims to describe the disk encryption feature on Cisco 8000 routers.  
Before IOS-XR 25.2.1, this functionality creates an encrypted partition where sensitive data such as configuration, certificates and keys are stored. This encrypted partition can be decrypted only using a specific key which is stored inside the Cisco 8000 Trust Anchor module (TAm) chip. Starting IOS-XR 25.2.1, the feature provides full disk encryption meaning all partitions are encrypted.

# How Does it Work?
Based on a Linux kernel, Cisco IOS XR leverages Linux dm-crypt infrastructure and additional components such Logical Volume Manager (LVM), Device-Mapper (DM), cryptsetup, Linux Unified Key Setup (LUKS); to transparently encrypt and decrypt data stored on the hard disk.

![8000-disk-encryption.png]({{site.baseurl}}/images/8000-disk-encryption.png)

# post IOS-XR 25.2.1

Starting IOS-XR 25.2.1, disk encryption is enabled by default, cannot be disabled, and encrypts all partitions:
-  <code>disk-encryption</code> command is not available anymore
- There is no longer seperate <code>/var/xr/enc</code> partition
- Instead, all partitions are now encrypted (rootfs, disk0, docker, harddisk, log etc)

Refer to [Cisco 8000 documentation](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/b-setup-and-upgrade-cisco8k/upgrade-software.html#disk-encryption) for upgrade workflows.

# pre IOS-XR 25.2.1

## Disk Encryption Configuration

By default, disk encryption feature is disabled. This can be confirmed with following output:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:8202-32FH-M_27#sh disk-encryption status
Fri Aug 16 23:54:26.500 UTC
Node:     node0_RP0_CPU0
--------------------------------------------------------------------------------
Mountpoint             Enc status             Mnt status             Prep status
--------------------------------------------------------------------------------
<mark>/var/xr/enc            Not Encrypted          Not Mounted</mark>            NA
</code>
</pre>
</div>

Additionaly, there is no secured partition mounted:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:8202-32FH-M#sh media
Mon Aug 26 13:47:23.471 UTC
Media Info for Location: node0_RP0_CPU0
Partition                            Size     Used  Percent    Avail
--------------------------------------------------------------------
rootfs:                             46.2G    19.1G      41%    27.1G
data:                               65.6G    12.8G      19%    52.9G
disk0:                               3.3G     4.5M       1%     3.1G
/var/lib/docker                      5.6G     720K       1%     5.3G
harddisk:                             51G      11G      22%      38G
log:                                 4.5G     491M      12%     3.7G
</code>
</pre>
</div>

Disk encryption can be enabled or disabled on-demand. Each change requires a system reload.

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:8202-32FH-M_27#disk-encryption ?
  activate    Activate disk encryption
  deactivate  Deactivate disk encryption
</code>
</pre>
</div>

**Note:** Once CLI is entered, the secured partition will be created/deleted and the router automatically reloads. System does not prompt for user confirmation.
{: .notice--info}

On Cisco 8100/8200/8700 fixed systems, <code>disk-encryption activate</code> must be executed in XR exec mode:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:8202-32FH-M_27#disk-encryption activate ?
  location  Specify location
RP/0/RP0/CPU0:8202-32FH-M_27#disk-encryption activate
Fri Aug 16 23:54:37.766 UTC
<mark>RP/0/RP0/CPU0:Aug 16 23:54:37.786 UTC: ssd_enc_server[271]: %OS-SSD_ENC-1-PREPARE_STARTED : Encryption activation prepare is in progress. Once it completes successfully, system will reload. Check encryption status by executing 'show disk-encryption status' CLI</mark>
Preparing system for backup. This may take a few minutes especially for large configurations.
	Status report: node0_RP0_CPU0: BACKUP INPROGRESS
RP/0/RP0/CPU0:8202-32FH-M_27#	Status report: node0_RP0_CPU0: BACKUP HAS COMPLETED SUCCESSFULLY
[Done]

[  OK  ] Stopped target XR installation and startup.
[  OK  ] Stopped target Multi-User System.
[  OK  ] Stopped target Login Prompts.
[  OK  ] Stopped target Containers.
snip
<mark>RP/0/RP0/CPU0:Aug 16 23:54:47.206 UTC: shelfmgr[236]: %PLATFORM-SHELFMGR-4-CARD_RELOAD : Reloading 0/RP0/CPU0: SSD encryption initiated card reload</mark>
[  OK  ] Stopped Permit User Sessions.
[  OK  ] Stopped target Remote File Systems.
Snip
[4017853.656053] pm-fpga: Turning Status LED to amber, Active, Alarm, Board and Chassis Attention LED to OFF …
[4017853.681085] pm-fpga: Power cycling local CPU power zone
</code>
</pre>
</div>

During reload phase, LVM cryptsetup and LUKS can be seen in action:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
Cisco 8000(R) Series BIOS Ver 1.09 Primary
Intel(R) Xeon(R) CPU D-1530 @ 2.40GHz
Board Type 0x111 PID 8202-32FH-M Serial FLM260902HU
X86FPGA 1.7.0 TamLib 5.01.01
Booting from Disk
  Booting `IOS-XR-latest'

Booting latest from Disk..
Loading Kernel..
snip
fstab modified mount options for 1 partitions
<mark>Setup encrypted LV...</mark>
<mark>Execute: cryptsetup luksOpen /dev/main-xr-vg/install-data-encrypted encrypted -d '-'</mark>
snip
[  OK  ] Found device Micron_5300_MTFDDAV480TDS partition01.
[  OK  ] Found device Micron_5300_MTFDDAV480TDS DR-Part.
[  OK  ] Found device /dev/mapper/encrypted.
[  OK  ] Found device /dev/main-xr-vg/install-data-scratch.
[  OK  ] Found device /dev/main-xr-vg/install-data-disk1.
[  OK  ] Found device /dev/main-xr-vg/install-data-docker.
[  OK  ] Found device /dev/main-xr-vg/install-data-log.
[  OK  ] Found device /dev/main-xr-vg/boot-lv.
[  OK  ] Created slice system-lvm2\x2dpvscan.slice.
         Starting LVM event activation on device 8:3...
         Starting File System Check on /dev/main-xr-vg/boot-lv...
         Starting File System Check…in-xr-vg/install-data-disk1...
         Starting File System Check…n-xr-vg/install-data-docker...
         Starting File System Check…main-xr-vg/install-data-log...
         Starting File System Check…-xr-vg/install-data-scratch...
<mark>         Starting File System Check on /dev/mapper/encrypted...</mark>
         Starting File System Check on /dev/sda1...
         Starting File System Check on /dev/sda2...
[  OK  ] Started LVM event activation on device 8:3.
[  OK  ] Started File System Check on /dev/sda1.
[  OK  ] Started File System Check on /dev/sda2.
         Mounting /mnt/dr_part...
<mark>[  OK  ] Started File System Check on /dev/mapper/encrypted.</mark>
         Mounting /var/xr/enc...
[  OK  ] Started File System Check on /dev/main-xr-vg/boot-lv.
         Mounting /boot...
[  OK  ] Started File System Check …in-xr-vg/install-data-scratch.
[  OK  ] Started File System Check …ain-xr-vg/install-data-docker.
         Mounting /var/lib/docker...
         Mounting /var/xr/scratch...
[  OK  ] Mounted /mnt/dr_part.
<mark>[  OK  ] Mounted /var/xr/enc.</mark>
[  OK  ] Mounted /boot.
         Mounting /boot/efi...
[  OK  ] Mounted /var/lib/docker.
[  OK  ] Mounted /var/xr/scratch.
[  OK  ] Mounted /boot/efi.
snip
SYSTEM CONFIGURATION COMPLETED
</code>
</pre>
</div>

## Disk Encryption Verification

Once the router has rebooted, disk encryption status can be seen as active:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:8202-32FH-M_27#sh disk-encryption status
Sat Aug 17 00 :44 :25.502 UTC
Node :     node0_RP0_CPU0
Mountpoint             Enc status             Mnt status             Prep status
<mark>/var/xr/enc            Encrypted              Mounted                NA</mark>
</code>
</pre>
</div>

The new encrypted partition is also present:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:8202-32FH-M_27#sh media
Sat Aug 17 00:44:44.968 UTC
Media Info for Location: node0_RP0_CPU0
Partition                            Size     Used  Percent    Avail
--------------------------------------------------------------------
rootfs:                             46.2G    10.3G      22%      36G
data:                               65.6G     9.3G      14%    56.4G
<mark>/var/xr/enc                          134M     3.8M       4%     120M</mark>
disk0:                               3.3G      15M       1%     3.1G
/var/lib/docker                      5.6G      13M       1%     5.3G
harddisk:                             51G     7.6G      16%      41G
log:                                 4.5G     463M      11%     3.8G
</code>
</pre>
</div>

## The Case of Dual RP
The disk encryption feature must be enabled on a node-by-node basis. This means both Route Processor (RP) must be encrypted for Cisco 8800 distributed systems or 8600 centralized systems running with redundant RPs.  

When using the ‘all’ keyword, both RP will be encrypted and reload, trigerring a chassis reboot:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP1/CPU0:8812-2#disk-encryption activate location all
Mon Aug 26 14:21:45.614 UTC
RP/0/RP0/CPU0:Aug 26 14:21:45.622280 UTC: ssd_enc_server[153]: %OS-SSD_ENC-1-PREPARE_STARTED : Encryption activation prepare is in progress. Once it completes successfully, system will reload. Check encryption status by executing 'show disk-encryption status' CLI
RP/0/RP1/CPU0:Aug 26 14:21:45.637364 UTC: ssd_enc_server[231]: %OS-SSD_ENC-1-PREPARE_STARTED : Encryption activation prepare is in progress. Once it completes successfully, system will reload. Check encryption status by executing 'show disk-encryption status' CLI
</code>
</pre>
</div>

Disk encryption can also be done on a per-RP basis. The active RP reload will then trigger a switchover to the standby RP:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/<mark>RP1</mark>/CPU0:8812-2#sh disk-encryption status location 0/rp1/CPU0
Mon Aug 26 14:12:22.167 UTC
<mark>Node:     node0_RP1_CPU0</mark>
--------------------------------------------------------------------------------
Mountpoint             Enc status             Mnt status             Prep status
--------------------------------------------------------------------------------
/var/xr/enc            <mark>Not Encrypted</mark>          Not Mounted            NA


RP/0/RP1/CPU0:8812-2#sh disk-encryption status location 0/rp0/CPU0
Mon Aug 26 14:12:33.819 UTC
<mark>Node:     node0_RP0_CPU0</mark>
--------------------------------------------------------------------------------
Mountpoint             Enc status             Mnt status             Prep status
--------------------------------------------------------------------------------
/var/xr/enc            <mark>Encrypted</mark>              Mounted                NA

RP/0/RP1/CPU0:8812-2#
</code>
</pre>
</div>

**Note:** In case of RP RMA, disk encryption must be launched again on the newly inserted RP.
{: .notice--info}

## Programmability
<code>Cisco-IOS-XR-linux-security-showenc-oper:disk-encryption</code> YANG path can be used to monitor encryption status:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
{
  "node_id_str": "Saturn-8711-32FH-M",
  "subscription_id_str": "app_TEST_200000001",
  "encoding_path": "Cisco-IOS-XR-linux-security-showenc-oper:disk-encryption/node",
  "collection_id": "759774",
  "collection_start_time": "1725279408203",
  "msg_timestamp": "1725279408205",
  "data_json": [
    {
      "timestamp": "1725279408204",
      "keys": [
        {
          "node-name": "0/RP0/CPU0"
        }
      ],
<mark>      "content": {
        "lv-mnt-pt": "/var/xr/enc",
        "enc-status": "Not Encrypted",
        "mnt-status": "Not Mounted",
        "prep-status": "NA"
      }</mark>
    }
  ],
  "collection_end_time": "1725279408205"
}
</code>
</pre>
</div>

# Conclusion
This article demonstrated how to activate disk encryption feature on Cisco 8000 routers pre IOS-XR 25.2.1. This functionality helps operating Cisco 8000 routers with the highest level of security.  
Starting IOS-XR 25.2.1, disk encryption is enabled by default, cannot be disabled, and encrypts all partitions.

# Acknowledgement
I’d like to thanks Rakesh Kandula & Dan Krezo for their review and feedback.
