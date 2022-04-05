---
title: TOF系统 第六章 电路系统
urlname: TOFSystem_C6
date: 2022-4-5 18:30:00
tag:  [TOF, 专题]
categories: TOF系统
photos:  https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-20.png
---

本章主要介绍TOF系统中的电路系统，包括iTOF和dTOF的电路系统，将使用两个实际TOF产品作为例子进行说明，其中dTOF系统将使用ST的VL53l5CX作为例子；iTOF系统将使用TI的OPT8241 3D Time-of-Flight Sensor以及配套的OPT9221 Time-of-Flight Controller和开发板电路进行介绍。由于作者本人不是电子工程专家，本章主要是在系统和模块级别进行一定的介绍和分析。

<!--more-->

---

回顾[TOF系统：第一章系统综述](https://faster-than-light.net/TOFSystem_C1/)，两种TOF系统的架构基本相同，主要都是由发射端和接收端构成（见下图）。而电子电路系统是实现系统架构的重要组成部分。比如发射端激光需要激光驱动芯片；接收端根据接收传感器的不同需要图像处理芯片对信号进行不同的处理，并直接进行一些片上计算输出深度信息；整个系统需要一个微控制芯片进行控制；需要一些储存单元储存固件及校准信息等。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-3.png)

而控制这些硬件电路的是固件Firmware，固件一般是以半永久的方式记录在硬件中的一种软件，在硬件断电后也不会消失；和Firmware进行交互的是该套系统的驱动driver/应用程序接口，该套程序可以让用户使用一系列更高级别的功能来控制固件的初始化，启动和停止测量距离，模式的选择等，可以通过I2C接口和Firmware进行交互；最上层就是用户自行开发的应用程序，调用系统的TOF测距的功能。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-1.png" style="zoom:50%;" />

# dTOF电路系统

我们以STMicroelectronics(意法半导体)的VL53l5CX作为例子进行dTOF电路系统的介绍

## 系统简介

VL53l5CX是ST出品的一款dTOF型测距模组，下图为实际模组图，正面图可以看到其发射端和接收端，中间是散热部件；背面图可以看到模组的电路连接端口和最中间的散热pad。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-2.png" style="zoom:40%;" />

**该模组的特点有：**

- 快速的测距能力

  - 多个zone的测距，可以选择4x4或者8x8zone
  - 低功耗待机模式
  - 最高400cm的测量距离
  - 单一zone内多目标探测距离能力
  - 60Hz帧率
  - 模组内的直方图处理和算法补偿以最小化和移除cover glass造成的串扰
  - 每个zone内有移动指示判断目标是否有位移
- 小型化和大FOV
  - 940nm VCSEL发射端，集成模拟驱动
  - 63° DFOV视场，使用DOE在发射端和接收端
  - 接收端是SPAD阵列
  - 低功耗的微处理器运行固件
  - 大小：6.4mm x 3.0mm x 1.5mm
- 易于集成
  - 单次回流焊组件
  - 灵活的供电选择，使用单一3.3V或2.8V控制，或者使用3.3V在AVDD和1.8V在IOVDD
  - 可以搭配多种范围的cover glass材质

**应用场景有：**

- 场景理解，多区域zone和多目标的识别能力可以用于机器人应用的3D室内空间映射和障碍识别。
- 大FOV和多区域扫描可以实现内容管理（卡车中的装货量，水箱，垃圾桶）
- 姿态识别
- 液面控制
- 视频投影仪的keystone校正
- 激光辅助对焦，增强相机AF自动对焦系统速度，增强若光照低对比度情况下的对焦鲁棒性
- VR、AR增强，双相机立体视觉和3D深度辅助
- 智能建筑和智能光照
- IoT（用户和物体检测）
- 视频对焦追踪，60Hz的帧率可以实现连续对焦算法

## 电路系统架构

上一节简述了模组的功能，模组的功能是要靠其系统实现的。下图展示了该dTOF系统的电路系统架构图，可以看到其主要的系统功能块和主要接口。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-3.png" style="zoom:30%;" />

### VCSEL Driver

VCSEL Driver用于驱动VCSEL工作，最简单的一种架构如下图所示，由于系统的控制部分是数字信号，VCSEL本身是模拟元器件，而且需要一个高电流，系统需要做的是快速的控制VCSEL的开关以发出高频脉冲光波。其中VCSEL的正极连接高电压，用于VCSEL本身的供电，该电压有时可以和数字电路部分的控制电压分离开来。VCSEL的负极端连接一个MOSFET（金属氧化物半导体场效应管），起到一个开关的作用。VCSEL本身可以被大致描述为一个有着特定大小电阻电容和电感的理想二极管。MOSFET的gate侧可以接收脉冲信号，来对MOSFET进行开关。对于高速应用，gate侧使用一个Gate Driver来提升信号速度，比如使用一对儿MOSFET来对VCSEL的MOSFET进行快速的充放电保证其开关高速性能。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-4.png)

### SPAD Detection Array

SPAD detection array执行的是检测任务，主要由SPAD像素点，淬灭和重置电路和读出电路组成。下图展示了一个8x8的SPAD阵列的单像素结构，其中SPAD像素点用于感收到光子时发生雪崩效应产生电信号；淬灭和充值电路进行电压控制，已完成先停止SPAD像素点的雪崩过程，再让其回到工作状态；而读出电路用于识别SPAD读出的信号，并最终输出时间差信号。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-5.png" style="zoom:50%;" />

读出电路中的一个重要组成部分是TDC(Time-to-Digital Converter)电路，为了实现时间相关单光子计数（TCSPC），整个系统要知道VCSEL发射出光子的时间和SPAD接收到光子的时间差。一个典型的TDC包含两个输入端一个输出端，其中输入端包含Start和Stop信号，在TOF应用中，Start信号可以为激光发射时控制信号的高电平，Stop信号为SPAD收到光子后输出的信号，TDC电路会记录两个信号的上升沿然后输出时间差。下图展示的是德州仪器的[TDC7200](https://www.ti.com/lit/ds/symlink/tdc7200.pdf)，可以最多识别开始信号到最多5个结束信号之间的时间差。而ST这个模组的TDC都集成在SPAD阵列周围。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-6.png" style="zoom:33%;" />

SPAD整体的设计难点是整个系统的工作速度，以及随着SPAD像素点增多后，如何提高SPAD像素点的fill factor以及控制整个模组的大小。下图展示了两种不同的SPAD array阵列的结构，不同于上面每个读出电路在单像素点周围（filling factor很小）。一种是读出电路整体在SPAD像素阵列边上（左图），一种是Sony的3D堆叠技术，整体读出电路在SPAD像素阵列下方（右图）。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-7.png" style="zoom:40%;" />

### Advance Ranging Core

这一部分的系统取决于不同公司叫法不同，主要包含的是对TDC出来的时间差信号，SPAD像素点地址信息等的信号处理。最主要的是片上histogram的计数，然后依据算法，计算出距离，并依据标定的各种参数对数据进行校准并完成最终输出。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C1/C1-7.png" alt="C6-7" style="zoom:80%;" />

### Micro Controller

Micro controller用于控制片上各子系统

### NVM，ROM，RAM

NVM用于来储存firmware，序列号，系统的校准信息等需要一直保存的信息，ROM用于储存系统初始化信息等，RAM用于系统运行的计算存储。

### 外围电路和端口

ST的文档中展示了配合该模组的外围电路，模组通过I2C端口（SCL，SDA，INT）和Host MCU进行通信进行输入控制信号和输出信号的传输。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-8.png" style="zoom:30%;" />

### 状态机

该模组系统有断电模式，低功耗待机模式，高功耗待机模式和测距模式。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-9.png" style="zoom:30%;" />



## 开发环境

想使用该模组进行开发，可以选用多种开发板，只要保证Host MCU上安装了该模组的驱动，以及外围电路连接正确即可。

下图展示了使用Arduino进行连接的方式

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-10.png" style="zoom:24%;" />

下图展示了使用STM32 Nucleo64单片机连接该模组的一种方式以及一个简单的输出形态

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-11.png" style="zoom:40%;" />

下图展示了某些开发人员在开发板上再连接显示屏进行输出

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-12.png" style="zoom:50%;" />

ST会提供面向不同平台的驱动API等，以及一些简单的示例程序。该模组很容易集成进一些电路系统，作为一个传感器进行工作。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-13.png" style="zoom:30%;" />

# iTOF电路系统

我们以Texas Instrument(德州仪器)的OPT8241 Chipset作为例子进行iTOF电路系统的介绍。其中OPT8241是iTOF传感器，需要搭配德州仪器的ToF controller OPT9221进行使用，OPT9221用于接收来自于OPT9241的数据进行深度信息的计算。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-14.png" style="zoom:40%;" />

OPT8241 Evaluation Module是德州仪器开发的用于OPT8241和OPT9221的评估开发板，包括Illlumination Board和Sensor Board，见下图。下图右图展示的是接收端Sensor Board的部分，其中重点标注出了传感器芯片OPT8241（在镜头下面），TOF控制芯片OPT9221以及一个Micron的DDR2，左图展示了加上了带有四个激光发射端的Illunimation Board的整体开发板布局。整体形成了一个iTOF系统。开发板包括一个5V电源插口和一个Micro-USB接口，可以直接连接电脑使用。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-15.png" style="zoom:40%;" />

## 系统简介

OPT8241特性如德州仪器官网介绍如下，它是一个QVGA的iTOF传感阵列

- 成像阵列：
  - 320 × 240 阵列
  - 1/3” 光学格式
  - 像素间距：15µm
  - 高达 150 帧/秒
- 光学属性：
  - 响应度：850nm 时为 0.35A/W
  - 解调对比度：50MHz 时为 45%
  - 解调频率：10MHz 至 100MHz
- 输出数据格式：
  - 12 位相位相关数据
  - 4 位共模（环境）
- 芯片集接口：
  - 与 TI 的飞行时间控制器 OPT9221 兼容
- 传感器输出接口：
  - CMOS 数据接口（50MHz DDR，16 通道数据、时钟和帧标记）
  - LVDS：
    - 600Mbps，3 个数据对
    - 1LVDS 位时钟对，1LVDS 采样时钟对
- 定时发生器 (TG)：
  - 可编程感兴趣区域 (ROI) 的寻址引擎
  - 调制控制
  - 抗混叠
  - 主/从同步操作
- 用于控制的 I2C 从接口
- 电源：
  - 3.3V I/O，模拟
  - 1.8V 模拟，数字，I/O
  - 1.5V 解调（典型值）
- 经优化的光学封装 (COG-78)：
  - 8.757mm × 7.859mm × 0.7mm
  - 集成光学带通滤波器（830nm 至 867nm）
  - 便于对齐的光学基准点
- 工作温度范围：0°C 至 70°C

**应用场景有**

- 深度感测：
  - 位置和临近感测
  - 3D 扫描
  - 3D 机器视觉
  - 安全和监控
  - 手势控制
  - 增强现实和虚拟现实

**OPT9221的特性如下**

- 最高120 FPS的控制能力
- 深度信息输出:
  - 12-Bit相位
  - 最高12-Bit强度信息
  - 最高4-Bit环境光
  - 过曝检测
- 芯片集接口：适用于OPT8241
- 输出（CMOS，8Lane数据，8个控制信号和时钟）
  - 数字视频协议适用：Data, VD, HD, Clock
  - 同步串行接口（SSI）适用
- 深度引擎:
  - 像素合并
  - ROI(Region of Interest)
  - 去混叠
  - 非线性校正
  - 温度校正
  - 高动态范围
  - 空间滤波
- 时钟协调:
  - 传感器控制
  - 主从同步
- I2C 接口
- 电源: 1.2-V Core, 1.8-V I/O, 3.3-V I/O, 2.5-V Analog
- 封装: 256-Pin, 9-mm × 9-mm NFBGA
- 工作温度: 0°C to 85°C

可以看到，由于iTOF使用的芯片更接近CMOS芯片，输出的信息量也比较大，所以OPT8241需要配合一个类似于ISP的OPT9221来工作。

## 电路系统架构

上一节简述了模组的功能，模组的功能是要靠其系统实现的。下图展示了该iTOF系统在评估开发板中的架构图，可以看到其主要的系统功能块和主要接口。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-16.png" style="zoom:50%;" />

### Illunimation Driver

发射端驱动用于驱动激光器，和dTOF的激光驱动没有本质区别，都是使用MOSFET等元器件实现，可以看到激光驱动的控制和调制信号来源自于OPT8241 Sensor，同时发射信号也输出给Depth Engine进行深度信息的计算。

### Sensor(OPT8241)

Sensor部分即OPT8241芯片，主要包括像素阵列，ADC和时间发生器。

像素阵列为320x240，最高支持150帧每秒，由于是iTOF应用，每帧读出4次强度信息(可能是使用的四相位连续波调制)，所以每秒可以读出600次数据。

ADC用来进行模拟到数字信号的转换

Timing Generator（TG）可编程，用于控制iTOF的调制，复位，读出，数字化序列。该可编程TG提供了对于不同算法的可能性（包括功率，移动物体的识别稳定性，信噪比和环境光降噪等）

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-17.png" style="zoom:50%;" />

### Depth Engine(OPT9221)

Depth Engine在iTOF系统中充当一个类似于电脑显卡的工作，快速的计算序列化的数据，然后输出简单的深度信息。可以看到它的核心是Depth Engine的计算单元，OPT8241 Sensor的数据和时钟通过LVDS(Low Voltage Differential Signaling) Receiver De-Serializer模块输入到Depth Engine。除了原始数据，Depth Engine还需要一些补充参数用于更准确的深度计算，包括在发射端靠近VCSEL的温度传感器的温度信息（我们提到过VCSEL温漂会造成波长变化进而影响深度计算的准确性），配置信息，时间控制信号等。

DDR/DDR2用于储存Sensor原始读出信息的储存，不在芯片里，需要连接一个使用，当所有必要信息（4相位的读出信息）都完成储存时，Depth Engine会读出这些数据进行深度信息的计算，TI推荐最小128mbits的DDR用于储存，使用DDR2可以比DDR更快进行数据读入读出，所以OPT9221配置了也可以和DDR2通信的控制器。

其他的模块是通信接口等模块。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-18.png" style="zoom:40%;" />

### Configuration EEPROM, Calibration EEPROM, USB Tx/Rx(FX2)

Configuration EEPROM用于存储其Firmware。

Calibration EEPROM用于储存校准参数，这些参数直接作用于输出后的深度信息矩阵上，所以不需要被Depth Engine读取，而是直接进行处理后完成最终输出。

USB Tx/Rx(FX2)用于USB的传输，其中开发板的固件存储在这个模块。

### Power management and monitoring

该模块主要是作为开发板本身的电源控制和监控。

## 开发环境

### 固件

该评估开发板包括两部分的固件fireware，一个是FX2固件和OPT9221的固件，这两个firmware分别存储在不同的EEPROM上。

### 软件

该开发板使用开源3D camera software development kit [Voxel-SDK](https://github.com/3dtof/voxelsdk). 为了使评估更容易，TI提供了一个基于该SDK开发的Voxek Viewer。该软件可以实现以下功能，软件GUI如下图所示：

- 实时查看以下数据: 相位，强度，环境光，距离，深度，点云
- 配置iTOF相机参数
- 基础统计：时间和空间平均，标准差，直方图
- 滤波器（时间和空间）：增加、删除、插入滤波器，配置滤波器参数
- 校准相机
- 更新OPT9221固件

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C6/C6-19.png" style="zoom:40%;" />

# 参考

**dTOF VL53L5CX**

1. [ST VL53L5CX](https://www.st.com/en/imaging-and-photonics-solutions/vl53l5cx.html)
1. [Arduino + VL53L1X](https://makersportal.com/blog/2019/4/10/arduino-vl53l1x-time-of-flight-distance-measurement)

3. [STM32 Nucleo64 board + VL53L5CX](https://www.st.com/resource/en/application_note/an5717-how-to-setup-and-run-the-vl53l5cxsatelusing-an-stm32-nucleo64-board-stmicroelectronics.pdf)

**iTOF OPT8241**

1. [OPT8241](https://www.ti.com/product/OPT8241?utm_source=google&utm_medium=cpc&utm_campaign=asc-null-null-gpn_en-cpc-pf-google-soas&utm_content=opt8241&ds_k=OPT8241&dcm=yes&gclid=CjwKCAjw0a-SBhBkEiwApljU0qyeBEc9poCDxuCffFwUhqPW2EaAmosHiLAu70C3ZRdWYrGzNG9dnhoC7gUQAvD_BwE&gclsrc=aw.ds)

2. [OPT9221](https://www.ti.com/product/OPT9221?utm_source=google&utm_medium=cpc&utm_campaign=asc-null-null-GPN_EN-cpc-pf-google-soas&utm_content=OPT9221&ds_k=OPT9221&DCM=yes&gclid=CjwKCAjw0a-SBhBkEiwApljU0p1sH64dcnOWczmyT83fdbqjvzMRyfE7JdKNr-bnl3c5dysC9cPL5hoCBV0QAvD_BwE&gclsrc=aw.ds)

3. [OPT8241 评估开发板文档](https://www.ti.com/lit/ug/sbou155b/sbou155b.pdf?ts=1649150588367&ref_url=https%253A%252F%252Fwww.google.com%252F)

4. [Voxel SDK](https://github.com/3dtof/voxelsdk)
5. [ Voxel Viewer User's Guide](https://www.ti.com/lit/ug/sbou157/sbou157.pdf?ts=1649076212391)

-----

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



