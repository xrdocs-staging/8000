---
published: true
date: '2022-08-23 16:07 +0200'
title: 'Cisco 8000 software manageability: USB boot'
author: Frederic Cuiller
position: top
tags:
  - iosxr
  - Cisco 8000
  - install
  - XR7
excerpt: >-
  This tutorial will describe and illustrate how to USB boot Cisco 8000 routers.
  This step-by-step procedure can be used to stage, re-image or recover a router
  from a boot failure.
---
{% include toc icon="table" title="Cisco 8000 software manageability: USB boot" %}

## Introduction

This tutorial will describe and illustrate how to USB boot Cisco 8000 routers. This step-by-step procedure can be used to stage, re-image or recover a router from a boot failure.  
Operation is a 2-part process:
- Creation of a bootable USB drive
- USB boot invocation

**Info:** Configuration will be erased. Content of harddisk: partition will not be erased during this operation. USB boot is different than factory-reset operation.
{: .notice--primary}

This procedure applies to all Cisco 8000 Series routers (8010, 8200, 8400, 8600, 8700, 8800) running IOS-XR , independently their Silicon One ASIC model. 

## USB Drive Preparation
A bootable USB drive is created by copying a compressed boot file into a USB drive. The USB drive becomes bootable once the content of the compressed file is extracted.

The first step is to download the compressed USB boot image from CCO. The same IOS-XR image is used for Cisco 8100, 8200, 8600, 8700 and 8800 series.
![boot-image.png]({{site.baseurl}}/images/boot-image.png)


The second step is to prepare the USB media.  
As prerequisite, storage capacity must be between 8GB (min) and 32 GB (max). USB 2.0 and USB 3.0 are supported. The disk must be formatted as FAT32 or MS-DOS file system using the Windows Operating System or Apple MAC Disk Utility. USB drive formatting is out of this article scope.  
![drive-formatting.png]({{site.baseurl}}/images/drive-formatting.png)


The ZIP file must be copied to the USB drive. It’s recommended to check file integrity with MD5 checksum verification:

<div class="highlighter-rouge">
<pre class="highlight">
<code>
{22-05-05 14:50}FCUILLER-M-X6F3:/Volumes/USB_8GB fcuiller% md5 8000-x64-usb-7.5.2.zip
MD5 (8000-x64-usb-7.5.2.zip) = <mark>5380c1dc23bd0484ac62338beabb5822</mark>
</code>
</pre>
</div>

![boot-md5.png]({{site.baseurl}}/images/boot-md5.png)

File must then be extracted on the USB drive, into the root folder:
![boot-extract.png]({{site.baseurl}}/images/boot-extract.png)

Which gives following structure:
![boot-structure.png]({{site.baseurl}}/images/boot-structure.png)

After extraction, USB drive can be ejected and is now a bootable drive.

**Info:** ZIP file is not required anymore after extraction on the USB drive. Keeping it or removing it will not alter the USB boot process described next.
{: .notice--primary}

## Plugging USB drive

Bootable USB drive must be inserted into 8100, 8200 or 8800 RP USB port.
Those are located on front panel for 8100 and 8200 series:  

![8100-usb.jpg]({{site.baseurl}}/images/8100-usb.jpg)
![8200-usb.jpg]({{site.baseurl}}/images/8200-usb.jpg)

with an exception for 8201 and 8202 (rear panel):  
![8200-q100-usb.jpg]({{site.baseurl}}/images/8200-q100-usb.jpg)

For 8800 distributed systems, USB drive must be inserted on active RP in any of the two USB ports:
![8800-rp-usb.png]({{site.baseurl}}/images/8800-rp-usb.png)

## USB Boot Invocation
There are two ways to invoke USB boot:
- From Boot menu
- From XR CLI

Both techniques ultimately lead to the same end-result.

### From Boot menu
It’s assumed operator doesn’t have access to XR prompt (no management access or credentials unavailable) for this operation. A console access is required (115200 baud, 8 data bits, 1 stop bit, No parity).   

Right after router’s power ON, ‘Esc’ key must be pressed mutliple times to pause the boot process and get access to the BIOS:
![boot-menu.png]({{site.baseurl}}/images/boot-menu.png)

If the GRUB menu appears instead (as shown below), whole operation must be repeated to gain BIOS access (power off, power on, break sequence):
![grub-menu.png]({{site.baseurl}}/images/grub-menu.png)

Once in BIOS menu, operator must navigate into Boot Manager section:
![boot-manager.png]({{site.baseurl}}/images/boot-manager.png)

Then select USB option and press enter:
![usb-boot.png]({{site.baseurl}}/images/usb-boot.png)

After this operation, USB boot process starts:
![usb-boot-start.png]({{site.baseurl}}/images/usb-boot-start.png)

Depending on USB drive speed, operation can take up to 10min. Once process is done, router reloads:
![usb-boot-end.png]({{site.baseurl}}/images/usb-boot-end.png)

**Info:** There can be multiple router reboots depending source/target software version pair.
{: .notice--primary}

Last, router reboots from local disk:
![boot-from-disk.png]({{site.baseurl}}/images/boot-from-disk.png)

As a fresh IOS-XR installation, a local administrator account must be created:
![local-account.png]({{site.baseurl}}/images/local-account.png)

*Et voila*:
![usb-boot-final.png]({{site.baseurl}}/images/usb-boot-final.png)

The whole process can be observed in this soundless screencast video (x4 speed):
<iframe width="560" height="315" src="https://www.youtube.com/embed/rmJywG1XXSI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### From CLI

When invoking USB boot with *reload bootmedia usb* CLI, it’s assumed operator has XR prompt access. This technique avoids BIOS menu access and directly triggers USB boot process.

<div class="highlighter-rouge">
<pre class="highlight">
<code>
Cisco-8000#reload bootmedia usb noprompt

Welcome to GRUB!!
Verifying (hd0,msdos1)/EFI/BOOT/grub.cfg...
(hd0,msdos1)/EFI/BOOT/grub.cfg verified using Pkcs7 signature.
Loading Kernel..
Verifying (loop)/boot/bzImage...
(loop)/boot/bzImage verified using attached signature.
Loading initrd..
Verifying (loop)/boot/initrd.img
</code>
</pre>
</div>

The exact same process will be executed: image copy, software installation, router reboot.

## Conclusion
This article described how USB boot can be used to stage, re-image or recover a Cisco 8000 router. While 2 different invocations techniques exist, operation results in a fresh and clean software configuration state.
