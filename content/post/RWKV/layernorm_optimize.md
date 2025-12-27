+++
date = '2025-07-12T22:46:37+08:00'
title = 'Layernorm_optimize'
draft = 'true'
description = ""   
categories = ["RWKV"]
image = ""
+++

## 优化setup worst Negative stack

### LN

400MHz，加入了打四拍模块，
当前setup WNS 0.257ns；Hold WHS 0.071ns  

## test_bt_ln.v

作为LN的顶层模块  
一开始addertree输入数量为4，流水深度为3  
已知addertree最大支持128个9位宽的数值相加，如果输入的小于等于128个9位宽的数，那么可以一次就行，如果大于128，那么需要用累加器，使用addertree计算多次。  
adder tree当输入数量为128时，流水深度为7

### 布局布线资源极少

综合的资源是正常的，但是布局布线的资源很少。  
这是因为代码中存在一些隐形问题不适配vivado，导致被vivado布局布线的时候sweep优化掉了，可以更改布局布线的策略，参考的网页如下：
[](https://blog.csdn.net/xinxulsq/article/details/80926720)  

### 流水线级寄存器

```verilog
always @(posedge clk or negedge rstn) begin
    // ...
    abs1_reg[j] <= abs1[j];
    abs2_reg[j] <= abs2[j];
    sin_reg <= sin_arr;
    // ...
end
```
这一步是将上一步处理好的所有绝对值和结果符号位，存入一组寄存器中，也就是“打了一拍”。这是一个非常重要的流水线设计，它的主要目的是：  
优化时序：将一个长的、复杂的计算路径（从输入到最终输出）切分成多个更短的路径，使得电路可以在更高的时钟频率下稳定工作。  
从总的计算步骤来看，它的确是变多了一级，总的Latency也确实增加了一拍。但它之所以能分割路径并优化时序，是因为它解决了数字电路中一个完全不同的问题：`单个时钟周期内的传播延迟`。  
--> 相当于一个很长的组合逻辑中间阶段一级，将前面的部分分配在一个时钟里面进行，另外一部分分配在下一个时钟里面运行，防止出现setup时间不足的情况。这样也就说明了可以将时钟频率拉高了，同时吞吐率也能增大，因为这是变成了两级流水。
