---
layout: post
title: "deep learning视觉应用概述"
description: ""
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

1998年，Lecun提出了卷积神经网络的算法应用于手写体识别(*Gradient-Based Learning Applied to Document Recognition*)  
其模型结构大致如下：

<img src="{{site.url}}/assets/CNN.png" width="800px" style="display:inline;" />

*卷积层*
每一个`feature map`选取一个filter(5*5)，一个filter共用其权重值W  
filter作为滑动窗口一样选取原图中的一定区域像素点进行线性运算(h=Wx)  
通过遍历整张原始图片，即可得到一个feature map  
设定多个feature map，作为对不同特征的提取  

*采样层*
对feature中的一个小区域(2*2)再进行聚合操作（线性运算，max等）   
得到一个更小的feature

多次的迭代上面两层（以上一层的输出作为下一层的输入）  
可以得到更高层次的特征，以这些特征进行有监督训练，可以明显地提升模型预测的效果

##Convolutional Boltzmann Machine

随着deep learning的流行，玻尔兹曼机的无监督预训练得到了广泛的应用  
在图像应用中，卷积算法与玻尔兹曼机的结合对unlabel image的特征提取取得了很好的效果  

Lee和Andrew Ng的论文(*Unsupervised Learning of Hierarchical Representations with Convolutional Deep Belief Networks*)  
用三层的玻尔兹曼机分别得到了人脸中不同层次的特征提取  

<img src="{{site.url}}/assets/deep-learning-graphics.jpg" style="display:inline;" />

##大规模无监督训练

很多研究者则通过提供更多的训练样本，训练更大规模的模型  

2012年，Stanford和Google的一次合作(*Building High-level Features Using Large Scale Unsupervised Learning*)，  
尝试用1000台机器，16000个核来进行集群计算，训练10亿个参数的网络  

而如此庞大的计算机集群研究者一般很难做到，因此提出了利用GPU进行高性能计算的方案  
hinton在(*ImageNet Classification with Deep Convolutional Neural Networks*)针对ImageNet中海量的图片   
用单机双GPU的方式实现了一个多重CNN，进行图片分类，其模型如下:

<img src="{{site.url}}/assets/ImageNet-CNN.png" width="800px" style="display:inline;" />

进一步改进的方案是使用多机多GPU的集群方式进行计算，但其中也涉及到一些难点：  
- GPU服务器之间的通信瓶颈
- 控制计算和多GPU通信的复杂算法设计

Coates和Andrew Ng(*Deep learning with COTS HPC systems*)使用了Infiniband进行集群机器的内部通信  
解决了以太网的数据传输瓶颈，同时利用MPI和GPU计算的框架，有效的简化了对集群数据通信的控制   
最终以3台4GPU的机器实现了2012年Google 1000台CPU集群的效果
