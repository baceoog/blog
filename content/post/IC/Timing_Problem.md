+++
date = '2025-07-11T16:09:37+08:00'
title = 'Timing_Problem'
draft = 'true'
description = "记录开发过程中遇到的时序问题"   
categories = ["RTL", "IC"]
image = ""
+++

## 时序问题

### 数据冒险(Data Hazard)

也叫流水线不同步（Pipeline Asynchrony）

### 时序违例（Timing Violation）

特指Setup/Hold Violation，是一个更底层的物理概念，指的是在单个时钟周期内，信号电平没有在规定的时间窗口内稳定下来，导致寄存器锁存了错误的数据。

