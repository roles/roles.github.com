---
layout: post
title: "deep learning视觉应用概述"
description: "deep learning视觉应用概述"
category: 
tags: ["deep learning"]
---
{% include JB/setup %}


##大规模无监督训练

很多研究者则通过提供更多的训练样本，训练更大规模的模型  

2012年，Stanford和Google的一次合作(*Building High-level Features Using Large Scale Unsupervised Learning*)，  
尝试用1000台机器，16000个核来进行集群计算，训练10亿个参数的网络  

而如此庞大的计算机集群研究者一般很难做到，因此提出了利用GPU进行高性能计算的方案  
hinton在(*ImageNet Classification with Deep Convolutional Neural Networks*)针对ImageNet中海量的图片   
用单机双GPU的方式实现了一个多重CNN，进行图片分类，其模型如下:

进一步改进的方案是使用多机多GPU的集群方式进行计算，但其中也涉及到一些难点：  
- GPU服务器之间的通信瓶颈
- 控制计算和多GPU通信的复杂算法设计

Coates和Andrew Ng(*Deep learning with COTS HPC systems*)使用了Infiniband进行集群机器的内部通信  
解决了以太网的数据传输瓶颈，同时利用MPI和GPU计算的框架，有效的简化了对集群数据通信的控制   
最终以3台4GPU的机器实现了2012年Google 1000台CPU集群的效果
