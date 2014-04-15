---
layout: post
title: "deep learning视觉应用概述"
description: "deep learning视觉应用概述"
category: 
tags: ["deep learning"]
---
{% include JB/setup %}

##神经网络算法

经典的神经网络算法在构造多层网络时，用梯度下降的方法，   
往往会在训练的过程中陷入到局部最小值难以跳出，   
而浅层结构又难以得到high level的特征   
同时，如果简单的将图像作为一个像素vector来输入，  
又会忽略了图像本身邻近像素点的关联性   

1998年，Lecun提出了卷积神经网络的算法应用于手写体识别(`Gradient-Based Learning Applied to Document Recognition`)  
其模型结构大致如下：

{% raw %}
<img src="/assets/CNN.png" width="800px" style="display:inline;"/>
{% endraw %}

##卷积玻尔兹曼机

随着deep learning的流行，玻尔兹曼机的无监督预训练得到了广泛的应用  
在图像应用中，卷积算法与玻尔兹曼机的结合对unlabel image的特征提取取得了很好的效果  

Lee和Andrew Ng的论文(`Unsupervised Learning of Hierarchical Representations with Convolutional Deep Belief Networks`)  
用三层的玻尔兹曼机分别得到了人脸中不同层次的特征提取  

{% raw %}
<img src="/assets/deep-learning-graphics.jpg" />
{% endraw %}

##大规模无监督训练

很多研究者则通过提供更多的训练样本，训练更大规模的模型  

2012年，Stanford和Google的一次合作(`Building High-level Features Using Large Scale Unsupervised Learning`)，  
尝试用1000台机器，16000个核来进行集群计算，训练10亿个参数的网络  

而如此庞大的计算机集群研究者一般很难做到，因此提出了利用GPU进行高性能计算的方案  
hinton在(`ImageNet Classification with Deep Convolutional Neural Networks`)针对ImageNet中海量的图片   
用单机双GPU的方式实现了一个多重CNN，进行图片分类，其模型如下:

{% raw %}
<img src="/assets/ImageNet-CNN.png" width="800px" style="display:inline;"/>
{% endraw %}

进一步改进的方案是使用多机多GPU的集群方式进行计算，但其中也涉及到一些难点：  
- GPU服务器之间的通信瓶颈
- 控制计算和多GPU通信的复杂算法设计

Coates和Andrew Ng(`Deep learning with COTS HPC systems`)使用了Infiniband进行集群机器的内部通信  
解决了以太网的数据传输瓶颈，同时利用MPI和GPU计算的框架，有效的简化了对集群数据通信的控制   
最终以3台4GPU的机器实现了2012年Google 1000台CPU集群的效果
