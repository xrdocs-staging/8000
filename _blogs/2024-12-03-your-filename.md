---
published: false
date: '2024-12-03 14:22 +0530'
title: ''
---
## Cisco 8000 QOS Archetecture


# Introduction

Cisco 8000 series routers combines industry leading Cisco Silicon One, IOS XR networking operating system (OS), clean sheet chassis design and Crosswork automation tools to deliver high performance routers empowered with programmable transport infrastructure and network provisioning & monitoring tools.

Cisco 8000 is powered by Silicon One Network Processing Unit(NPU) which is evolving since 2019 , started with 10.8T capable 1st generation and 51.2T capable 3rd generation already rolled out in deployment.

Cisco 8000 router series has 3 form factors, Fixed, Centralised and Distributed Line card systems. there are many variants with different throughput capacity and port configurations already shipping in the field. There are different Cisco 8000 router variants addressing different roles in the network primarily Service Provider domain and Webscaler / Data Center domains. 

System buffering capability is one of the important architecture piece in NPU when it get designed to cater webscaler/DC and Service Provider use cases. Primarily 2 types of buffering needs during congestion situations in networks:  shallow buffering to absorb inflight packets over short range fiber links and deep buffering to absorb inflight packets over long distance fiber links. 

This document is targeted to help those readers who is interested in understanding below topics,
- SiOne NPU architecture
- Cisco 8000 QOS scheduling and buffering
- Cisco 8000 shaping & buffering behaviour on Bundle ports
- Cisco 8000 policing behaviour and its aggregate behaviour on Bundle ports



