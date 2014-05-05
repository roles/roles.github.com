---
layout: post
title: "卷积神经网络GPU实现（四）：卷积层反馈"
description: ""
category:
tags: ["deep learning"]
---

卷积层反馈主要的任务是根据已经求得的feature map(`z`)的倒数，推导出对filter中各权重值的导数，如下图：

<a href="/assets/convnet_dE_dw_k_1.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_1.png" style="margin-top:20px; width:80%; height:80%" /></a>

对卷积filter权重(w[0][0]和w[7][7]为例)的导数计算为：

<a href="/assets/convnet_dE_dw_k_8.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_8.png" style="margin-top:20px; width:50%; height:50%" /></a>

<a href="/assets/convnet_dE_dw_k_9.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_9.png" style="margin-top:20px; width:50%; height:50%" /></a>

相应的图示为，**32\*32红色区域对应项相乘再求和**：

<a href="/assets/convnet_dE_dw_k_2.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_2.png" style="margin-top:20px; width:50%; height:50%" /></a>

<a href="/assets/convnet_dE_dw_k_3.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_3.png" style="margin-top:20px; width:50%; height:50%" /></a>

- 每个block是8\*8\*8的结构，负责计算16个filter权重值的导数，按threadIdx.z分为8层，每层是一个8\*8的plate，负责相邻两个filter的计算（**即每个线程负责两个相邻filter的同一位置点的计算**）
- 每个plate是8\*8，刚好与filter的大小对应，因此plate中每一个线程负责filter中一个权重值的计算

下面以w[0][0]和w[7][7]的导数计算为例说明：

- 对一张图片的计算分8次进行，每次隔4行将11行的image数据载入到share memory中去
- 由于share memory的大小为16\*4，因此一次计算又分两次载入feature map的导数到share memory中

<a href="/assets/convnet_dE_dw_k_4.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_4.png" style="margin-top:20px; width:80%; height:80%" /></a>

<a href="/assets/convnet_dE_dw_k_5.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_5.png" style="margin-top:20px; width:80%; height:80%" /></a>

数据载入完成后，即可针对share memory中的部分进行对应相乘求和的计算

w[0][0]的导数计算：
<a href="/assets/convnet_dE_dw_k_6.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_6.png" style="margin-top:20px; width:80%; height:80%" /></a>

w[7][7]的导数计算：
<a href="/assets/convnet_dE_dw_k_7.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/convnet_dE_dw_k_7.png" style="margin-top:20px; width:80%; height:80%" /></a>
