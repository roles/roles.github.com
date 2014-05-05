---
layout: post
title: "卷积神经网络GPU实现（五）：采样层反馈"
description: ""
category:
tags: ["deep learning"]
---

采样层反馈的任务是采样层的逆过程，**将8\*8的dE\_dy\_i扩展到32\*32的dE\_dy\_h(乘1/16)**

###数据录入

由于在dE\_dy\_i的计算中用到了cublas库，因此**dE\_dy\_i是column major的**  

- dE\_dy\_i的内存布局为**numCases \* numFilters \* 8 \* 8**，这里不妨假设numFilters=16
- grid的布局与dE\_dy\_i是一致的，**gridDim.x = numFilters \* 8 \* 8**，**gridDim.y = numCases / 16**
- block结构是16 \* 16，**一张图片的计算会分在4个block中进行**（如图所示, 8\*8=16\*4）

<a href="/assets/de_dy_h_1.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/de_dy_h_1.png" style="margin-top:20px; width:80%; height:80%" /></a>

如上图所示，在数据的录入过程中，每个block会将其在dE\_dy\_i所对应的数据载入到share memory中(shDerivs)

###扩展计算

- 每个block的计算的都会涉及到16张图的1/4的计算
- 计算的过程中，**threadIdx.x方向相邻的4个thread为一组**共同进行shDerivs中4个点的扩展计算（如下图黄色的部分涉及到image1,5,9,13）
- 在一次计算中，4个thread分别将shDerivs中的一个点扩展到对应image中的4列
- 蓝色部分表示tidx为60-63的4个线程，它们与黄色部分都在处理同一张image

<a href="/assets/de_dy_h_2.png" class="folio-image-wrapper"><img class="bordered" alt="screenshot" src="/assets/de_dy_h_2.png" style="margin-top:20px; width:80%; height:80%" /></a>
