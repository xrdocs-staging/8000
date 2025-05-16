---
published: true
date: '2025-05-16 16:46 +0200'
title: Cisco 8000 Packet Generator
author: Frederic Cuiller
position: top
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
This article aims to document Cisco 8000 and Silicon One packet generator functionality. The built-in packet generator can be used for troubleshooting purposes or to perform network diagnostics without the need of external traffic generator.

# Implementation

Cisco 8000 Packet Generator leverages Silicon One ASIC NPU Host. This component is typically used for OAM features (TWAMP, SRv6 IPM, BFD, CFM etc.). NPU host is built with its own Network Processor Engine (NPE) and can access all NPU slices.
