---
title: TOF系统 第三章 iTOF系统
urlname: TOFSystem_C3
date: 2021-10-07 20:00:00
tag:  [TOF, 专题]
categories: TOF系统
photos: https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-0.jpg
---

第一章中我们介绍了TOF的基本原理，应用，系统架构和分类。第二章中，我们详细介绍了dTOF系统，系统原理，架构和零部件性能出发，分析重要的系统参数。本章，我们会用同样的形式介绍iTOF系统。阅读本章前，请阅读第一章。

<!--more-->

---

# iTOF基本原理和核心组件

如第一章的介绍，iTOF的原理是测量发射的正弦波/脉冲信号与接收的正弦波/脉冲信号之间的相位差的方法计算出时间。iTOF可以按发射光波的调制方式分成连续波调制（CW-iTOF）和脉冲调制（Pulsed-iTOF）。下图一展示了iTOF的系统架构，发射端iTOF主要也使用VCSEL，但是有一些应用里，iTOF可以使用LED。激光或LED的光线经过diffuser匀光片照射被测场景。接收端使用传感器进行接收。dTOF和iTOF在传感器端的区别是iTOF使用的是CMOS工艺开发的CIS传感器(Camera Image Sensor)以及配套电路，而dTOF需要使用SPAD传感器。CW-iTOF和Pulsed-iTOF在硬件架构上没有很大区别，主要是控制和传感电路的一些调制解调电路区别。下图二展示了一个实际的iTOF模组（由Leica和pmd联合研发），可以从外观上看到发射端的diffuser和接收端的镜头。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-1.png" style="zoom:67%;" />

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-2.webp" style="zoom:50%;" />

iTOF的主要优势是和dTOF比起来相对简单低速的硬件电路，可以实现更高的帧率，以及空间分辨率，同时模组成本也更低；但是iTOF有着测量精度随距离增加下降，多路串扰以及相对比较高功耗等问题。

## 激光驱动

Laser/LED Driver是发射端电路的主要部分，负责驱动激光或者LED，控制电路发出调制信号，控制光源发射。发射调制的信号可以是脉冲型或者连续波型。

## 激光器或LED

在iTOF中，根据不同应用场景的模组，选择使用VCSEL或者LED。和LED相比，激光器的调制速度快很多，高频LED的3dB roll-off 频率一般为10MHz到30MHz，而VCSEL的3dB roll-off 频率一般高于100MHz。iTOF主流还是使用VCSEL。因为调制速度越高，深度测量的精度就越高，我们会在后面的系统参数部分列出公式。下图展示了LED和激光调制能力的对比。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-3.gif)

## 发射端光学

iTOF发射端一般直接使用diffuser进行光束整形，让光源均匀照射被测场景，diffuser一般是玻璃基底上的规则或者非规则微结构，将VCSEL阵列发出的光进行均匀化，最后形成top hat形式的光束。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-4.png)

## 接收端光学

和dTOF系统一样，接收端光学零部件需要的主要是收集期望测量的视场角内的光线，这种镜头F数越小越好，相对照度越高越好，同时镜片上可以进行一定的镀膜增强透光性，同时接收端会使用尽量窄带的滤光片，去滤除其他波长的杂光干扰。

## 接收传感器CMOS

iTOF接收端是基于CMOS Image Sensor工艺制程的，主要使用的是 (CAPD)像素工艺。如我们第一章介绍，iTOF是一个调制解调信号的过程，为了能更准确的计算发射信号和接收信号的相位偏移，使用Current-Assisted Photonic Demodulator（电流辅助光子解调CAPD）像素工艺，该种工艺可以同步取样发射光和接收光信号。同时CAPD允许在每一个像素的光电二极管中加载可转换方向的偏转电压，制造一个漂移电场，这样可以把光信号产生的电子拉向两个不同的半导体结。如下图所示，VCSEL发射出来的光，进入传感器，被转换成电子，每个像素点在不断转换的电压的辅助下，持续输出成两个信号，通过这两个信号，就可以计算相位偏移，

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-5.gif" style="zoom:67%;" />

以Sony的IMX556为例：

- 背照式CMOS工艺
- 640 x 480像素
- Global Shutter
- 10.0um 像素大小
- 1/2"传感器大小

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-6.jpg" style="zoom:50%;" />

提供iTOF的传感器供应商有 [Sony](https://www.sony-depthsensing.com/), [Infineon](https://www.infineon.com/cms/en/product/sensor/tof-3d-image-sensors/), [Melexis](https://www.melexis.com/en/product/MLX75027/Automotive-VGA-Time-Of-Flight-Sensor), [Analog Devices](https://www.analog.com/en/applications/technology/3d-time-of-flight.html), [EPC Photonics](https://www.espros.com/), [Samsung](https://www.samsung.com/semiconductor/minisite/isocell/vision-sensor/tof-sensor/), [Artilux](https://www.artiluxtech.com/explore)等。

## 电路

iTOF不需要特别的额外电路，传感器输出的信号经过一定的处理后和发射信号进行对比即可计算出相位偏移，但是为了实现比较好的效果，算法比较复杂。iTOF的CMOS输出是模拟信号，为了进行数字处理，还需要进行模数转换电路，但是ADC电路本身就非常普遍。dTOF直接输出的就是数字信号。

---

# 系统参数

## 探测距离

电子消费品的iTOF的探测距离一受限于精度，比如1%的精度，在10米就是10cm的深度误差。所以到底多远的距离，取决于应用。电子消费品的iTOF应用，一般只能达到5m左右的探测距离。同时，由于iTOF采用泛光透射，传感器可以探测到的能量随距离降低的速度很快，随着距离增加，信噪比降低到一个程度下就完全无法区分调制信号和背景噪声了。

## 探测距离的精度和误差

iTOF的测量精度和量程存在矛盾，同样相位解析精度下，测量精度反比于调制频率。但是高调制频率会降低测量量程，造成距离测量含糊混叠，不过这种混叠可以通过一些方式解决，比如多频率调制去歧义。我们在这里具体讨论这几个问题。当我们通过算法得到相位偏移ϕ后，距离d就如下公式所示，fmod为调制频率。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-7.png)

实际中，由于光子散粒噪声，读出电路噪声和多径干扰等问题，相位估计ϕ会有误差ϵϕ，那么距离估计误差ϵd如以下公式所示，可以看到，通过增加调制频率，可以降低距离估计误差。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-8.png)

下图也很好地展示了为什么提高调制频率可以很好地降低距离误差。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-9.jpg)

**误差**

产生相位估计误差的原因主要有：时钟精度，发射端电路调制精度，VCSEL的调制脉冲速度，接收传感器的精度，读出电路噪声等。比如发射和接收端的同步信号发生了10ps的偏移，就相当于1.5mm的位移。这些需要在后面的标定和校准过程中进行补偿。

## 距离歧义

但是，使用高调制频率会造成距离估计的歧义。由于iTOF是通过计算相位后计算距离的，相位是在0-2π中变化的。根据公式，如果调制频率是100MHz，那么每1.5m，相位0-2π完成一次变化，如果是200MHz，就是每0.75m完成一次变化。假设100MHz调制频率，测出一个相位偏移是2π，那么距离可以是1.5m, 3m, 4.5m等等。解决距离歧义的一种方法是多频率调制，如下图所示，可以使用几个不同的调制频率去确认哪个距离才是真实的距离，这种方法低调制频率的部分可以提供无歧义的距离估计，而高调制频率的部分可以提供更高的精度，兼顾了测量距离和测量精度的需求。但是增加了计算量降低了帧率。最终的深度估计是通过不同调制频率得到的距离给上不同的权重进行计算，更高的频率的权重更高。如果最优选择各个调制频率的权重，深度识别误差是反比于调制频率的均方值rms的。对于一个固定的深度误差需求，增加调制频率可以降低积分时间或者降低发射光强。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-10.png" style="zoom:50%;" />

## 噪声和抗干扰能力

**多路径干扰**

多径干扰(Multi-Path Interference，MPI)是光线通过多个路径返回传感器，如下图所示。iTOF无法分辨哪个信号是直接返回的信号，哪个信号是经过多次反射返回的信号。学术界和工业界尝试多种方法去解决多路径干扰，但是这是iTOF原理本质上问题，无法完全根除。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-11.jpeg)

**飞点噪声**

飞点噪声是深度信息图里出现的不连续的无效深度信息，如下图所示，a图示深度点云图，红色代表距离更近，蓝色代表距离更远；b图示强度值，c图是振幅值。飞点噪声是下面的白圈中展示的情况，形成原因是在深度变化较大的位置（不连续），物体反射率不是很好，传感器同时接收到前景和后景反射回来的能量，这两个能量叠加在一起，使原始数据中包含了两部分的信息，于是计算深度时就无法得到准确的深度值。下图中左上角的白圈里的飞点噪声是另一种现象，深度信息错误是由于显示屏上部的高反射造成的。同时镜头散射，像素间的串扰等问题也会引起飞点噪声。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-14.png)

## 空间图像分辨率

现阶段电子消费品iTOF使用的sensor多为QVGA或者VGA(0.3MP，10um像素大小)，三星在2021年初的一个会议论文中推出了1.2MP的iTOF传感器。

## 功耗

iTOF需要采用高频调制发射相对较高功率的激光，相对于dTOF，平均功率要高。

## 工艺分析

对于i-ToF的发射端，前面我们分析了，如果想提高精度，就要提高调制频率，所以需要高频率高功率调制的驱动芯片。这种芯片和VCSEL的温度系数与 i-ToF 测量的温漂紧密相关，需要尽可能保证线性。同时，在接收端，虽然iTOF的传感器没有SPAD传感器制造起来难，但是也需要有足够的灵敏度，动态测量范围，足够的像素尺寸。对于普通的图像传感器，需要增加2TAP或者4TAP的采样和控制电路。

## iTOF的标定和校准

iTOF校准一般来说需要进行以下几种校准：镜头校准，频率校准，非线性校准，相位共偏校准 ，逐像素校准，温度校准 。如下图所示，展示了校准的过程，温度校准参数独立于其他几个校准流程。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C3/C3-15.png)

**镜头校准**

镜头校准主要校准镜头的光学中心和畸变等问题，使用张正友标定算法进行。

**频率校准**

设定的调制频率和实际产生的频率是有误差的，需要校准。

**非线性校准**

发射端的光波不是理想方波或者正弦波，会产生误差，这种误差表现为测量得到的相位差，与理想的相位差之间有偏差，这个偏差呈现一定的变化规律，在2pi的相位范围内，呈现误差先变大再变小的规律。见上图中非线性校准中的绿线和红线，绿线是一个圆代表理论相位，红线是实际相位。这种误差可以使用插值查表法进行非线性误差补偿。主要是对于波长范围内，均匀选取多个位置，记录真实距离和深度测量值，然后进行插值，得到对应相位的表数据，在测量到这个相位的时候，就对该相位进行补偿。这种校准也叫Wiggling标定校准或Cyclic error calibration。

**共偏校准**

完成前几步校准后，进行共偏校准。共偏校准是确定图像中心的相位偏移，然后基于已知确定的距离进行校准。

**逐像素校准**

逐像素校准是校准非中心的像素距离测量值。完成共偏校准后，测量一个平面时，可能会得到一个不是很平的有起伏的深度点云，这时就需要进行逐像素的校准和补偿。这时整个校准的最后一步。也叫Pixel dependent offset calibration。

**温度校准**

由于温度变化导致测量产生相位偏移并造成误差的影响因素有两个：光源和接收传感器。光源端，发射波长随着温度的升高会发生红移现象。计算深度的波长和实际波长不一致，这就会导致检测的深度值出现误差。接收传感器端，存在由于温度上升导致测量出的深度值也变大的情况，这种和温度线性相关的现象称为温飘。这种温度补偿通过测量不同温度下的TOF性能，发射及接收的参数，计算出补偿参数，写入模组。实际模组通过使用NTC测量温度，在不同温度下使用不同的温度补偿系数。

---

# 参考模组

以下列出一些参考模组，供读者研究学习

[Analog AD-96TOF1-EBZ开发板](https://www.analog.com/en/design-center/evaluation-hardware-and-software/evaluation-boards-kits/AD-96TOF1-EBZ.html?icid=ToF-AD-96TOF1-EBZ-research)

[Analog ADSA3100传感器](https://www.analog.com/en/products/adsd3100.html#product-overview)

[光鉴科技Stellar 200](http://www.deptrum.com/site/product_details/40)

[飞芯电子](http://www.abax-sensing.com/pro-3.html)

[光微科技](http://www.tof3d.com/index.html)

[Omron B5L](https://components.omron.com/sensors/3d-tof-sensor-module)

[Texas Instrument OPT8241](https://www.ti.com/product/OPT8241#description)


-----

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



