---
layout: post
title:  "大信号放大--运放的关键参数"
category: 技术
tags: OP-Amp
---
    参考资料：TI学习资料SBOA126--Large-Signal Specifications for High-Voltage Line Drivers.pdf

###介绍：
对于一个运放，其带宽和信号失真性能在整个输出信号的摆幅范围内不是恒定的。
在高速运放领域中，主要有两种架构（电流反馈和电压反馈），选择电流反馈(CFB)运放还是电压反馈(VFB)运放主要取决于所需要的带宽、摆率、增益和输出电压摆幅。

###电压反馈
电压反馈运放主要使用增益带宽积(GBWP)来描述：即电压增益和信号带宽的乘积是固定的（上限），但是高速VFB在低增益下却不会严格遵守这一规律，这是由于低补偿和寄生参数影响了单位增益带宽[1]，如下图中，G=1时计算的增益带宽积为800MHz，而G=10时计算的增益带宽积为300MHz，与理论值280MHz更为接近。

![interpreter pattern](/public/upload/Big-signal-parameters/f1.jpg)


###电流反馈
电流反馈运放并没有增益带宽积这一指标。在VFB中，决定补偿的反馈参数(feedback factor)包含了两个增益控制电阻(gain-setting resistors: noninverting gain or the noise gain)。而CFB的反馈参数仅包含反馈电阻(feedback resistor)。这种架构最终导致了CFB的增益和带宽之间的无关性:

![interpreter pattern](/public/upload/Big-signal-parameters/f2.jpg)
 
所以，虽然CFB的单位增益带宽比VFB的单位增益带宽小很多，但是当G=10时，CFB的带宽又能够超越VFB的带宽好几倍！

当然，上述分析都是基于小信号应用的分析，如果是大信号放大，则影响带宽的主要因素为运放的摆率！小信号带宽描述了大信号带宽的上限，具体能够实现的大信号带宽则由摆率限制。同时，CFB运放不仅能够在高增益下支持高带宽，同时也能比传统的双极性输入VFB达到更高的摆率！另一方面，一些slew-boosted VFB中已经开始使用了CFB的架构，但是这些运放依然受GBWP的限制。

VFB和CFB更深入的比较可以参考Ref. 2

###摆率
摆率是一个针对大信号的参数，应当将其与两个小信号参数：上升时间和下降时间加以区分。

比如：THS3091在G=5，输出20V时的摆率为7300V/μs，所以输出电压摆动20V所需要的时间为2.7ns(20V/7300V/μs=2.7ns)。然而，实际输出电压摆动20V所需要的时间要大于2.7ns，因为摆率是在输出范围的25%~75%（或10%~90%）的范围内测得的，即对于一个脉冲信号，摆率是在其上升沿中部测量的，而不是整个脉冲的上升时间：

![interpreter pattern](/public/upload/Big-signal-parameters/f3.jpg)

 
对于一个大信号正弦波，已知摆率和输出信号幅度，可以计算该运放所能处理的最高频率：

![interpreter pattern](/public/upload/Big-signal-parameters/f4.jpg)

	  
当摆率无法支持信号频率时，op放大器将会进入开环状态（运放无法将误差信号减小）。此时，fmax被称为“无摆率限制带宽”

需要注意的是“无摆率限制带宽”和3dB带宽是不同的，“无摆率限制带宽”可以很明显地在输出信号波形中观察出来（正弦波?三角波）。

然而，上述计算“无摆率限制带宽”的方法似乎并没有计算3dB带宽的方法那样普适，因此，原始的SR=2πf*Vp可以改写为：

![interpreter pattern](/public/upload/Big-signal-parameters/f5.jpg)

 
通常情况下，摆率只考虑信号80%的部分（10%~90%），因此可以用因子0.8来修正摆率的计算式：

![interpreter pattern](/public/upload/Big-signal-parameters/f6.jpg)

 
正是由于这种测量区间所带来的影响，由上式所计算的摆率通常并不能表示运放的摆率上限。为了达到80-dBc的失真水平，应当原则摆率比最大信号情况高20倍的运放！

###对于仿真的警示：
由于仿真软件使用的都是元件的线性模型，而且电路的频率响应都是使用无限小的输入信号来计算的，因此ac扫描并不能描述op放大器的任何大信号频率响应。时域分析和冲击响应的仿真则能够更好地反映大信号情况下的现象。

###输出电压摆幅和输出电流
为大信号放大选择op放大器时所需要考虑的最后一个重要因素为输出电压的摆幅和输出电流。

由于op放大器的输出电阻在电压输出时需要从供电电源处获取一部分分压，因此限制了输出摆幅（运放的双轨？？）。同时，运放的输出电压摆幅也受运放的输出电流限制：

![interpreter pattern](/public/upload/Big-signal-parameters/f7.jpg)
 
###总结：
摆率、输出电压摆幅、输出电流都对运放的带宽和失真有着很大的影响！

###参考资料：
[1] Ramus, X. (2009). Making the most of a low-power, high-speed operational amplifier. Texas
Instruments application report SBOA121.
[2] Karki, J. (1998). Voltage feedback vs current feedback op amps. Texas Instruments application report SLVA051.
