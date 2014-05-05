---
layout: post
title: "卷积神经网络GPU实现（二）：采样层"
description: ""
category:
tags: ["deep learning"]
---

采样层的任务是将32\*32的feature map每一个4*4的块求其均值映射到sample map上

block的结构为32\*8，每一个block负责一个feature map的归约计算，  
在threadx方向上，**每四个thread共同负责一个4\*4块的归约计算**

<a markdown="1" href="/assets/convnet_sampling_1.png" class="folio-image-wrapper">
<img markdown="1" class="bordered" alt="screenshot" src="/assets/convnet_sampling_1.png" style="margin-top:20px; width:80%; height:80%" />
</a>

以左上角黄色的四个thread为例：

1. 四个thread分别将4\*4块的4列归约到第一行，  
2. 然后载入到share memory的第一列，全部线程都完成后，64\*4的share memory载入完成，  
3. 然后针对share memory将四行归约到第一行，即可得到64个subsampling target，  对应到8\*8的sampling map上

<a href="/assets/convnet_sampling_2.png" class="folio-image-wrapper">
<img class="bordered" alt="screenshot" src="/assets/convnet_sampling_2.png" style="margin-top:20px; width:60%; height:60%" />
</a>
