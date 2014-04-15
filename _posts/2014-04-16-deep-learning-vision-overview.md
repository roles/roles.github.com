---
layout: post
title: "deep learning视觉应用概述"
description: "deep learning视觉应用概述"
category: 
tags: ["deep learning"]
---
{% include JB/setup %}

进一步改进的方案是使用多机多GPU的集群方式进行计算，但其中也涉及到一些难点：  
- GPU服务器之间的通信瓶颈
- 控制计算和多GPU通信的复杂算法设计

Coates和Andrew Ng(`Deep learning with COTS HPC systems`)使用了Infiniband进行集群机器的内部通信  
解决了以太网的数据传输瓶颈，同时利用MPI和GPU计算的框架，有效的简化了对集群数据通信的控制   
最终以3台4GPU的机器实现了2012年Google 1000台CPU集群的效果
