---
published: true
date: '2025-05-16 11:27 +0200'
title: Cisco 8000 FIB Scale for Silicon One P100
author: Frederic Cuiller
tags:
  - Cisco 8000
  - Silicon One P100
  - FIB scale
position: hidden
excerpt: >-
  This article aims to document and demonstrate FIB scale and capabilities for
  Cisco 8000 routers running Silicon One P100 ASIC.
---
# Introduction
Similarly to the work done on Cisco 8000 running Silicon One Q100 and Q200 ASIC, this article aims to document and demonstrate FIB scale and capabilities for Cisco 8000 routers running Silicon One P100 ASIC.

This article is applicable for the following products:

- Cisco 8711-32FH-M: 1RU, 12.8Tbps, with MACsec
- Cisco 8212-48FH-M: 2RU, 19.2Tbps, with MACsec
- Cisco 88-LC1-36EH: 28.8Tbps LC, without MACsec
- Cisco 88-LC1-12TH24FH-E:12Tbps LC, without MACsec
- Cisco 88-LC1-52Y8H-EM: 3.7Tbps LC, with MACsec

# Silicon One P100 FIB
The FIB structure and implementation is similar to Silicon One Q100 and Q200 ASIC. The main difference is the LPM scale.


