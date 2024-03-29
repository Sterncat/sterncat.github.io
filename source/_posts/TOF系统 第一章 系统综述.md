---
title: TOF系统 第一章 系统综述
urlname: TOFSystem_C1
date: 2021-08-26 23:45:00
tag:  [TOF, 专题]
categories: TOF系统
photos: https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-0.jpg
---

新开一个专题用于介绍近几年非常流行的TOF系统，这个系列目的是给读者提供一些由浅入深了解TOF系统的文章。内容会包括TOF系统综述；iTOF和dTOF的具体介绍，系统参数，优劣势，算法等；TOF系统中的各个零部件的介绍和特性，如VCSEL，发射和接收镜头，接收端传感器CIS/APD/SPAD/SiPM，驱动电路ASIC等。

<!--more-->

---

# TOF简介

## 基本原理

TOF，Time of flight的简写，直译为飞行时间，大部分应用场景是指光学TOF，是测量光在介质中飞行一定距离耗费的时间来进行距离测量的方法。基本原理可以说非常简单，如下图所示，一个光源，发射一束光，记录发射时的时间点，光照射在目标上反射回来，被接收器接收到，记录接收到反射光的时间点，两者的时间差为t，距离d = 光速c * t /2。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-1.png" style="zoom:50%;" />

---

## TOF应用

TOF应用场景非常多，电子消费品领域有人脸识别，照相机辅助对焦，临近传感器，体感互动，手势识别，AR等，扫地机器人避障，无人机避障，3D场景扫描等等；工业和安防领域可以用于自动机器人，人数统计，智能停车场，智能交通，智能仓储，提及测量等；智能驾驶领域的激光雷达LIDAR也属于TOF的范畴（不过我们不在该系列文章中具体讨论）。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-2.png)

---

# TOF系统架构

为了实现基本原理描述的距离测量，一个典型的TOF系统由以下几个部分组成：

**发射端Tx：**包括激光光源，主要是VCSEL；驱动激光的驱动电路ASIC；控制光束的光学零部件，可能是准直镜头或者衍射光学元件等，滤光片。

**接收端Rx：**接收端包括接收端的镜头，滤光片；传感器，根据不同的TOF系统，可能是CIS、SPAD、SiPM等；专门的图像处理芯片ISP用以处理海量的接收端芯片信息；

**电源管理：**VCSEL需要稳定的电流控制，SPAD需要高电压等，都需要较好的电源管理。

**软件层：**包括固件，SDK，OS和应用层等。

可以看到激光光束从VCSEL产生，通过光学元件后射向空间中，有一些光线被目标物反射并由接收端接收到，通过计算这中间的时间，可以获得距离或深度信息。该系统架构里并没有覆盖噪声光路，比如阳光产生的噪声，或者多次反射后返回接收端的光线造成的多径噪声，我们会在后面的内容里具体讨论噪声的课题。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-3.png)

**TOF实际模组**

根据使用场景和应用不同，TOF系统和模组的大小可能有很大差异，但是系统架构都基本统一，如下图所示，左边是AMS的TMF8805的TOF临近传感器，大小是2.2mm x 3.6mm x 1.0mm，可以看到发射端的类似矩形的孔和接收端的镜头；右边是Leica和PMD出品的TOF参考设计“Holkin”，大小类似于一个手机照相机镜头模组，可以看到发射端是采用了VCSEL+diffuser的形式，接收端是一颗镜头。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-4.png)



下图展示的是微软的Azure Kinect深度相机，可以看到发射端和接收端，发射端为了美观在产品最外侧使用红外透过可见光截止镀膜的保护盖板挡住。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-5.jpg" style="zoom:50%;" />

---

# TOF系统分类

本节介绍TOF的各种系统以及实现原理，TOF系统常见的分类方式是从获取距离的具体方法进行分类，主要分成dTOF（直接TOF）和iTOF（非直接TOF）。这两种方法在硬件和算法上有很大区别。我们在系统分类这里先简单介绍系统的不同原理，在后面再具体讨论系统优劣势的比较和挑战。讨论dTOF和iTOF的区别前，我们先看看TOF系统主要面临的问题，两种系统的相同点，然后我们可以看到这两种系统是如何通过不同的方法解决这些问题的，进而就可以理解两种系统的区别。

虽然TOF的原理听上去很简单，发出一个光脉冲，然后检测什么时候返回了一个光脉冲，通过计算这个中间经过的时间，就可以知道距离。但是传感器本身是一直在进行接收能量的，如何知道传感器在某个时间点t接收到的光就是发出去的光呢？

1. 发射足够亮的光（相对于噪声）以达到足够的信噪比，传感器接收到高于环境光的能量就可以知道这是发射出去的光。如果是电子消费品，用于生活环境中，自然环境光强范围十分大，波长范围也很广，同时一个电子消费品很难发射非常强的光强，一方面要考虑激光安全，要根据IEC60825的要求限制激光的能量，另一方面也要考虑耗电的问题。所以选择合适的波长发射光十分重要。
2. 找一个应用场景中不存在的波长或者能量最低的波长，这样就可以在这个波长发射相对弱一些的光脉冲，然后传感器前面放置窄带滤波片，这样，传感器只能收到这个波长的光。
3. 对发射的光进行编码，让其有可以识别的特征，比如发射光亮暗亮暗亮暗，接收端接收到这个形式的光的时候，就知道这是发射出去的光了，这就像黑夜中的SOS手电筒闪烁。

太阳光的光谱如下图所示，黄色为大气层上方的太阳光谱，红色为经过大气吸收后的太阳光谱，在近红外区，可以看到有氧气和水蒸气的吸收峰。这些吸收峰的波长位置理论上都可以作为TOF系统应用的波长。大部分TOF应用都会选择850nm和940nm，一方面这两个波长的发光源器件可以使用VCSEL实现，再长的波长可能需要EEL实现；另一方面，接收端传感器对850nm是最敏感的，可以提供最佳的信噪比，940nm的感光度会比850nm低，如果波长更长，传感器的制造更难，电子消费品中很少选择。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-6.png" style="zoom:50%;" />

---

## dTOF

director time of flight，直接测量距离法，顾名思义，是直接测量光子飞行时间的方法。关键元器件是**单光子雪崩二极管**(Single Photon Avalanche Diode, SPAD) ，简单来说该种传感器具有探测单个光子的灵敏度，只要有一个光子击中SPAD，SPAD就会最终输出一个脉冲。SPAD输出这个脉冲后，可以很快复位，准备接收下一个光子。

可是即使在850nm和940nm，环境光也是存在的，这意味，只要SPAD开始计数，就可能不停的有光子会撞击SPAD并令其激活并输出脉冲，那如何分辨接收到的光子是发射端发出，被目标物反射回来的这个光子呢。首先要使用合适的窄带滤波+衰减，让大部分场景的环境光不能透过滤光片，以降低噪声，但是在850nm的衰减滤波是固定的，不可能满足在所有光照场景下，刚刚好环境光强度不会激发SPAD，然后发射光能量刚好有光子进入；而且依赖于滤光片和衰减，测量误差可能非常大。

dTOF使用的方法叫**时间相关单光子计数法**(TCSPC，Time correlated single photon counting)，名字听起来很复杂，实际上就是“数光子出现的时间”。具体见下图，假设我们使用发射端VCSEL快速发射100个激光脉冲，记录SPAD每次收到脉冲的时间Δt，我们就有了100个Δt，见左图；把这些不同Δt出现的次数画成直方图，其中某个时间段的Δt出现频率最高，就是最可能的物体位置造成的Δt，而其他的Δt出现的地方可能是SPAD自己的噪声，环境光噪声，光线多径反射后返回传感器的噪声等。在实际应用中，VCSEL可以达到非常高的调制脉冲速度，VCSEL的每个脉冲宽度可以在ps到ns级别；SPAD也可以实现非常快的相应速度。举一个例子，实际模组中，可以实现用200ns发射100个1ns的脉冲，占空比50%；每1ms进行一次这样的发射，得到一个距离结果；每10ms求10次结果的平均反馈给操作系统使用；如此该TOF可以实现100次每秒的测距。

在传感器端，可以使用多个SPAD传感器去增加测量结果的准确性，每个传感器点可以镀膜去实现不同的衰减，用以降低环境光噪声。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-7.png)

---

## iTOF

indirect time of flight，非直接测量法TOF，主要方法是测量发射的正弦波/脉冲信号与接收的正弦波/脉冲信号之间的相位差的方法计算出时间，也叫phase-based TOF。在这种TOF系统中，光子的飞行时间t，是关于相位差的函数。因为是测量光强，所以iTOF的硬件可以使用普通的图像传感器架构，传感器输出的是接收到的光强和时间的函数，通过对比该函数和发射端的信号的函数，可以计算出飞行时间。需要注意的是，普通图像传感器的特点就是在一个固定时间收集光子，然后转化成电信号输出。

iTOF再细分，可以按发射光波的调制方式分成连续波调制和脉冲调制，常见的连续波调制使用正弦波信号，常见的脉冲波调制使用方波信号。请注意，如果对所有TOF在发射光波调制方式上进行分类，有些公司和研究机构把dTOF也称之为一种脉冲调制型TOF，不过笔者建议只在iTOF中讨论调制方式的分类。

- 使用连续波作为调制信号的被称为连续波调制（CW-iTOF）
- 使用脉冲作为调制信号的被称为脉冲调制（Pulsed-iTOF）

**连续波调制(CW-iToF)**

如下图所示，灰色正弦曲线是调制的发射的正弦波信号，黑色正弦曲线是接收到的信号（可以看到强度下降了），这两个信号中间有一个时间差，于是产生了一个相位偏移ϕ（0-2π），波长λ = c/ƒ，ƒ为调制频率，于是Δt = ϕ * λ，由此距离d就如图所示。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-8.png)

在接收端，需要做的事情就是确定相位偏移了多少，本质上是一种相位解调方法。对于iTOF，使用相同间隔的积分时间，让传感器每1/4个波长的发射时间内进行积分，接收光线，得到光强数据I1，I2，I3，I4，由此可以得到相位值：

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-9.png)

由于相位值在0-2π范围内，所以只靠一次测量，iTOF系统是无法分辨接收到的信号是延迟了ϕ还是ϕ+2π，所以iTOF会用各种方法的测量去消除这种测量同义性，我们会在后面的章节具体讨论。

**脉冲调制（Pulsed-iTOF）**

脉冲调制是使用方波作为调制信号，在接收端进行信号读取，确认接收到的信号和发射信号的相位偏移。我们看一下iTOF是如何通过这种像素点计算出来相位偏移ϕ的。如下图所示，四个方波分别为发射信号，接收信号，一个传感器A以和发射信号同样频率的波形进行工作，和发射光光波同步进行光信号读取积分。一个传感器B以和发射信号同样频率的波形进行工作，但是在时域上有π的相位偏差，进行光信号读取积分。可以看到传感器A和传感器B在交替开关对接收信号进行积分，他们分别接收到了返回信号的一部分。输出的强度信号B/(A+B) * π既是发射信号和接收信号的相位差ϕ。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-11.png)

实际iTOF传感器是使用类似于照相用图像传感器进行光强的测量，但是成像型图像传感器在进行测量时，是有积分时间，读出时间，静止时间的，所以采样的中间是有间隔的，这无法满足iTOF原理的需求。所以用于iTOF的传感器会在传感器电路上进行一些改变，以一个像素可以实现连续采样的功能。普通的传感器一个像素点（Pixel）对应一个输出，而iTOF的传感器一个像素点对应两个输出（如下图所示），在一个pixel上面接两个开关，一半时间用电流到A端口，一半时间到B端口，这样A端口在进行光子接收时，B端口在这个时间片段内可以进行各种模拟电路的准备操作等，待A端口完成信号接收，断开电路后，B端口进行信号接收，A端口可以用这段时间进行信号输出等事项。这两个端口就可以对应前面的传感器A和传感器B。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-10.png)

再接近实际一些，这些传感器上使用的不是开关，而是电压去操纵像素点里的电流流动，这种被称为电流辅助光子解调像素点(current assisted photonic demodulator pixel)，简称CAPD，我们在后面的章节会进行更具体的说明。下图([链接](https://thinklucid.com/tech-briefs/sony-depthsense-how-it-works/))比较形象的展示了脉冲调制型iTOF的原理。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-13.gif)



---

## dTOF和iTOF比较

dTOF和iTOF各有优劣，这需要更具体的了解系统参数，本章主要基本原理介绍，我们在后面的章节具体介绍iTOF和dTOF时再进行更具体分析介绍以及从系统参数的角度出发去比较不同种类TOF系统的优劣。

---

## 按信息获取维度分类

如果从芯片可以获取信息的程度上，或者说输出信息的复杂性上进行分类，可以分成**1DTOF，2DTOF，3DTOF**。1DTOF最简单的形态就是一个单点测距的TOF系统，可以只有一个单像素点的芯片，只返回一个距离值。2DTOF最常见在扫地机器人上，可能是有一束激光来回扫描一个区域，然后传感器读取每个位置的距离值，扫地机器人只关心房间下层空间的距离，所以只面前需要一条线上的距离信息。3DTOF就是使用一个2维传感器阵列和成像镜头结合，每个像素对应的是某个视场的距离，这样，整个传感器可以输出一个3D信息。

-----

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



