+++
date = '2025-07-09T21:56:37+08:00'
title = 'XringO1'
draft = 'false'
description = ""   
categories = "xiaomi"
image = ""
+++



## 玄戒O1芯片架构

### 芯片基础

***CPU:*** 基于ARMv9.2指令集架构的Cortex CPU集群  
***GPU:*** ARM的Immortalis GPU IP  
***系统级设计:*** 玄戒O1芯片的互联技术采用了ARM的CoreLink系统IP
![SoC Architecture](https://raw.githubusercontent.com/etherealzoom/blog_img/main/Pic/20250709222546138.png)

### CPU架构

1. 十核四丛集设计  
Cortex-X925 3.9GHz *2  
-> 最高主频达3.9GHz，表现超越业界标准设计，是小米玄戒团队通过大量创新和数百次版图迭代优化的结果  
Cortex-A725 3.4GHz *4  
Cortex-A725 1.9GHz *2  
Cortex-A520 1.8GHz *2  

2. Ultra-fast Micro Control Unit-on chip  
在硬件上实现对各个核的调度

3. 缓存结构  
*数据保存的位置离使用的位置越近，性能越高，降低延迟核功耗*  

4. 玄戒团队三个提高主频的方法  
(1) 优化供电网络设计：边缘供电+空间组网  
玄戒团队将主供电单元移到SoC两侧，缩短逻辑单元的物理距离  
(2) 自研多种3nm标准单元Std Cell  
(3) 电路版图全链路迭代优化+人工精调

### NPU

玄戒O1内置6核NPU，集成Scalar标量加速器、Vector矢量加速器和Tensor张量加速器，NPU算力可达44 TOPS ，支持多算法并行计算。在拍摄时，可并行“拍照”和“预览”算法，不仅满足计算摄影的影像算法需求，更针对小米自有的端侧模型进行了硬件底层的深度定制。  
配合全新第三代小米端侧模型，AI处理速度更快，同时功耗更低，使手机在智能语音助手、图像识别、智能场景优化等AI应用方面表现更加出色。  

### ISP

xiaomi自研第四代ISP单元

***References.***  
[1] [极客湾](https://www.bilibili.com/video/BV18sJHz5ENR/?share_source=copy_web&vd_source=9ffe3740cc87c3869a8fd1a2a7aa9ac5)  
[2] [老石谈芯](https://www.bilibili.com/video/BV1yB7aztECc/?share_source=copy_web&vd_source=9ffe3740cc87c3869a8fd1a2a7aa9ac5)