---
layout: post
title: "deep learning视觉应用概述"
description: "deep learning视觉应用概述"
category: 
tags: ["deep learning"]
---
{% include JB/setup %}

##神经网络算法

经典的神经网络算法在构造多层网络时，用梯度下降的方法   
往往会在训练的过程中陷入到局部最小值难以跳出  
而浅层结构又难以得到high level的特征   
同时，如果简单的将图像作为一个像素vector来输入   
又会忽略了图像本身邻近像素点的关联性  

1998年，Lecun提出了卷积神经网络的算法应用于手写体识别(`Gradient-Based Learning Applied to Document Recognition`)  
其模型结构大致如下   

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
