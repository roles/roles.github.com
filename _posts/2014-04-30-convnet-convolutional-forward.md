---
layout: post
title: "卷积神经网络GPU实现（一）：卷积层"
description: ""
category:
tags: ["deep learning"]
---

###存储结构

原图是32\*32大小的图片，有3个颜色channel，存储格式为：

<a href="/assets/convnet_y_h_1.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_y_h_1.png"style="margin-top:20px; width:80%; height:80%" /></a>

filter为8\*8大小的块，为了使经过卷积层之后的feature map也是32\*32便于后面的计算  
在原图的基础上进行了补白(padding)：

<a href="/assets/convnet_y_h_2.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_y_h_2.png"style="margin-top:20px; width:40%; height:40%" /></a>

###数据录入

grid的结构是numCases\*numFilters，即**每一个block负责一张图片针对一个filter的卷积计算**  
block的结构是32\*8，share memory中保存图片的数组shImg结构是39\*16，因此数据的录入和计算的过程都分为四个部分，如下图所示（阴影为载入share memory的部分）：

<a href="/assets/convnet_y_h_4.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_y_h_4.png"style="margin-top:20px; width:70%; height:70%" /></a>

###计算过程

分别以左上角的**threadA(threadIdx.x=0, threadIdx.y=0)**和右下角的**threadB(threadIdx.x=31, threadIdx.y=7)**的计算和对应原图和share memory的关系来说明：

<a href="/assets/convnet_y_h_3.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_y_h_3.png"style="margin-top:20px; width:100%; height:100%" /></a>

- threadA在四个子过程分别计算图示中feature map中的四个点，share memory中始终对应左上角8\*8的区域，对应到原图为四个阴影块的计算
- threadB同理为对应的右下角
