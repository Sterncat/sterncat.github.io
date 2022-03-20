---
title: TOF系统 第五章 VCSEL
urlname: TOFSystem_C5
date: 2022-2-05 18:00:00
tag:  [TOF, 专题]
categories: TOF系统
photos:  https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-3.png
---

本章主要介绍TOF系统中的重要零部件VCSEL，介绍其基本原理，结构，制程，封装，主要参数，产业链等，为不熟悉VCSEL的读者提供一个了解VCSEL的初始信息和基本体系结构的了解，可以作为一个更深入了解VCSEL的入门信息。

<!--more-->

---

# VCSEL简介

垂直腔表面发射激光 Vertical Cavity Surface Emitting Laser (VCSEL)，是一种光电元器件，是一种半导体激光器件。由于其有各种优良的特性，近些年在通信，电子消费品，工业，汽车，医疗领域得到的广泛的应用。尤其是电子消费品领域，VCSEL作为光源出现在各种应用中，包括临近传感器，距离传感器，面部识别，3D相机等。2020年75%的VCSEL产值是在传感领域，24%的产值在通信领域。

## VCSEL, LED, EEL对比

常用的半导体光源有三种，分别是VCSEL，LED（light-emitting diode）和边发射器 edge-emitting laser（EEL）。从下面的图和表格我们来比较他们的不同。

- 这三种光源都是使用半导体制程制造，其中LED和VCSEL发光从表面垂直方向发出，EEL从侧面发出，这种模式决定了LED和VCSEL都可以在Wafer上进行片上测试，而EEL要把Wafer切开后形成单个零部件封装后才能进行测试，由此，成本上EEL最高。
- 光强上，EEL可以提供单一元件的最强光强，所以常用在光通信领域。单一的VCSEL和LED光强受限于本身的几何结构，但是可以使用阵列的形式提供高强度光强。
- 光谱宽度方面，VCSEL和EEL都是受激发光，属于激光，都有很窄的光谱宽度。LED属于发光半导体，光谱宽度较宽。
- 光束质量上，VCSEL可以提供相对小发散角的光束，而LED发光是Lambertian，EEL发出的是椭圆形光束，常需要光束整形。
- 调制速度上VCSEL和EEL都可以达到相对高速，所以常用于光通信领域，同时VCSEL的高调制速度使其可以使用在TOF应用中。LED相对较慢。
- 光电转换效率上，VCSEL对比LED有着更高的效率，所以也受电子消费品领域的青睐。
- 封装方式上，VCSEL和LED由于是表面发光，可以使用多种表面元器件通用的封装技术，而EEL受限于侧面发光，封装方式需要特别设计。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-1.png)

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-2.png)



## VCSEL形态

如果按一个die上有几个发光点去分，VCSEL可以分成单点，多点和阵列式，如下图所示（请注意，下图的不同VCSEL的放大比例不同，单点的VCSEL其实很小，相当于阵列VCSEL的一个点的大小）

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-3.png)

下图展示另一个单点VCSEL，左图展示了有1个点和4个点的VCSEL，每个发光点被称为Mesa。右图展示了一个单点VCSEL，可以看到中间的发光点，该VCSEL包括1个anode和3个cathode触点，可以看到Mesa本身的宽度和厚度的比例。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-4.png)

VCSEL十分小巧，下图来自于II-VI公司的[宣传图](https://ii-vi.com/vcsel-technology/)，可以看到单点VCSEL和一个蚂蚁头部的对比。VCSEL是上面的薄层，下面都是wafer的本身基底（substrate）。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-5.jpg" style="zoom: 20%;" />

阵列型的VCSEL如下图所示，该VCSEL已经封装在一个基板（submount）上并通过Wire bond进行了电路的连接。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-6.png)

# VCSEL结构

首先，我们先复习一下最简单的激光器模型。激光器是利用受激辐射原理使光在某些受激发的物质中放大或振荡发射激光的器件。典型的激光器包括1.增益媒介 2.激光泵浦源 3.高反射镜（理想情况100%） 4.输出耦合（99%反射镜） 5.激光光束。3和4构成了谐振腔。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-7.png" style="zoom:33%;" />

下图展示了VCSEL最基本的模型，作为一种激光器，它的基本模型和典型激光器没有区别，只是实现不同结构的方式是通过半导体制造的方式实现的。其中激光的谐振腔由两个分布式布拉格反射器（distributed Bragg reflector，DBR）构成，相当于两个反射镜，这个结构是在由高、低折射率介质材料交替生长成的，每层都是发射激光的四分之一波长。常见的VCSEL中，上下两层DBR结构会被注入p型和n型材料，形成半导体pn结，这样电流就可以流过。激光的量子阱在这两层DBR中间，常见的是3~5层。最上面是金属电极，中间红色是出光位置。下层是n型基底和最下层的金属电极，同时下层金属电极也可以作为反射镜增加出光效率。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-8.png" style="zoom:33%;" />

下图展示了更贴近实际的VCSEL的结构和形态

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-9.png" style="zoom:40%;" />

下左图展示了各层的材料。可以看到最中间是三层InGaAs材料形成的量子阱，中间两层GaAs材料分隔开，整个激光激发区的外侧由AlGaAs形成的限制区夹住。限制区/氧化层/氧化孔径是限制光束和电流的区域，是由侧向氧化方式实现的。右图重点展示了氧化层的位置和形态，以及如何限制光路和电流的，电流和光束正常情况下都无法穿过氧化层（电流增大会穿过氧化层，因为很薄）。再外侧是上下分布式布拉格反射器，由掺杂p型材料的30个1/4波长周期AlGaAs/GaAs构成其中一个反射镜，一般反射率会在99.5-99.9%之间，由掺杂n型材料的17.5个1/4波长周期的AlAs/GaAs构成另一个反射镜，一般反射率为99%。然后是两侧的高浓度掺杂的p+GaAs层和n型GaAs基底以及最外侧的金属接触点。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-10.png)

VCSEL可以设计成上面变出光或者下表面出光，如下图所示，上面的VCSEL是基底在下，上方出光，而下方的VCSEL是设计成从基底侧出光，同时基底一面被做上了微透镜阵列，这种形式的好处是直接可以对光束进行整形，或者改变光束的方向。不过现阶段常见的VCSEL还是第一种。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-11.png)

# VCSEL制程

前面一节简单介绍了VCSEL的结构，VCSEL制造就是要实现这种结构。VCSEL主体结构是多层的“千层饼”结构，基于多层主体结构中，再继续进行限制氧化层，台面，电极等结构的制造，以实现光，电，热的设计需求；也会进行绝缘保护等工序，实现更好的寿命和信赖性。

VCSEL的主要制程被分成两个主要的部分，一部分是实现千层饼结构的**MOCVD(metal organic chemical vapor deposition)金属有机物化学气相沉积技术**了；一部分是实现后端各种结构和需求的**FAB晶圆工艺**，其中FAB晶圆工艺包括光刻，电极蒸发沉积及剥离，湿法台面蚀刻，侧向湿法氧化，BCB填充等。

我们使用下图作为例子来具体说明，图1为实现的VCSEL的形态，左边为我们前文就看到的侧视结构图，右图为实际的VCSEL俯视图，可以看到p和n电极；

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-12.png)

图2为制造流程。图片引自[Vertical-Cavity Surface-Emitting Lasers: Large Signal Dynamics and Silicon Photonics Integration](https://www.semanticscholar.org/paper/Vertical-Cavity-Surface-Emitting-Lasers%3A-Large-and-Haglund/ae7e69b921743cd8645734f546039c943ffa5656)

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-13.png" style="zoom:60%;" />

## 晶圆MOCVD外延

MOCVD，metal organic chemical vapor deposition，金属有机物化学气相沉积是在基板上成长半导体薄膜的一种方法（是一种外延技术，其他外延技术还有分子束外延，液相外延，固相外延），是实现VCSEL千层饼结构的第一步。MOCVD是一种外延技术，主要实现的是在晶圆上一层一层的物质。在MOCVD制程中，反应气体在升高的温度下在反应器中结合以引起化学相互作用，将材料沉积在基板上。在MOCVD中，将超纯气体注入反应器中并精细计量以将非常薄的原子层沉积到半导体晶片上。含有所需化学元素的有机化合物或金属有机物和氢化物的表面反应为晶体生长创造条件，形成材料和化合物半导体的外延。不同于传统的硅半导体，这些半导体可以包含的组合III族和V族，II族和VI族，IV族或第IV族，V和VI族的元素。 外延工艺的难点在于精准控制每层外延层的厚度、均匀度，以及控 制缺陷密度。由于每层外延层只有几纳米至几十纳米厚度，所以对于 工艺精细度的要求非常高。

对于VCSEL，一般为在GaAs或InP晶圆上生长多层反射层与发射层。

## (a) p极触点制造

p极触点一般触点材料是 Ti/Pt/Au，使用电子束蒸发或者金属蒸镀后剥离方式实现。

## (b) 第一台面蚀刻并进行SixNx沉积

完成极点的沉积后，进行第一台面蚀刻，把上层的DBR反射部分和激发层暴露出来。这步蚀刻是一个化学过程，比如使用ICP-RIE方法结合NF3及SlCl4进行蚀刻。完成蚀刻后，一层SixNx被PECVD方法被沉积在整个台面上，这层SixNx可以在后面的侧向氧化制程中保护Mesa表面和芯片表面。

## (c) 侧向氧化

首先侧面的SixNx会被移除，然后进行侧向氧化的制程，该制程是VCSEL制造中非常重要的一步。湿法氧化工艺氧化工艺中，外延结构中高铝组分氧化层通过湿法氧化工艺后变成低折射率、高绝缘性的Al2O3形成有效的光场和电场限制。（在高温水蒸汽中将 AlAs 层氧化，变为有绝缘性的 AlxOy 层，其折射率也大大降低，因而成为把光、载流子限制在垂直方向的结构。）氧化孔径的大小和形状影响着VCSEL器件的很多性能参数，如VCSEL的阈值电流大小、光功率大小、串联电阻大小等。湿法氧化工艺时，通过控制氮气气体流量、腔室内加热温度来控制氧化速率，保证氧化速率的稳定性，从而达到用时间精准控制氧化孔径大小的目的，同时还使用红外光源的CCD用于实时观察氧化情况，保证氧化工艺的成功率。VCSEL的湿法侧向氧化孔径的大小和形状影响着VCSEL器件的很多性能，一般该限制孔径的大小为4~20um。VCSEL氧化限制层的横向结构形状通常为圆形，所以VCSEL的光斑基本为圆形。

侧向氧化后，为了不让氧化制程的AlAs层继续向内氧化影响谐振腔体积，造成激光特性突变，一些VCSEL很多还会增加一步绝缘保护制程(passivation)，使用ALD方式，沉积跟VCSEL氧化层特性接近的氧化铝（Al2O3）薄膜，这种侧面镀膜均匀，致密性高，厚度很薄就可以完全绝缘保护芯片，应力也小。

## (d) 第二台面蚀刻

完成侧向氧化孔径后，使用SiCl4和ICP-RIE工艺进行蚀刻，形成第二台面。保护的SixNy层被ICP-RIE结合NF3移除。

## (e) n极触点制造

n极触点为Ni/Ge/Au，使用电子束蒸发或者金属蒸镀后剥离方式实现。完成后在惰性N2环境中进行退火（430℃）。

## (f) 接触层剥离

Mesa和n触点外的接触层被进一步剥离，以降低bondpad（接触pad）的电容。

## (g) BCB填充

使用喷溅式镀膜方式将一层BCB（苯并环丁烯）喷射到整个VCSEL芯片上，使整个VCSEL平面化。

## (h) Bondpad沉积

沉积Ti/Au的bondpad使其和n/p极接触点连接。

**不同VCSEL种类可能步骤多多少少有些区别，但是基本都是类似于上述流程，其中外延，侧向氧化和绝缘保护制程是关键工艺。**

# VCSEL封装

完成VCSEL制造后，整体晶圆将经过检测，dicing进行出货。VCSEL在模组端会进行封装，主要会进行Die attach和wire bonding。Die attach的主要目的是将VCSEL和模组基板建立导电和导热通道，如下图所示。对于不同功率的VCSEL，使用的bonding环氧树脂种类不同，以实现不同的散热和信赖性需求。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-14.png" style="zoom:30%;" />

完成基底bonding后，会进行wire bonding，单一VCSEL发光点一般使用一条wire bonding即可，但是对于高功率VCSEL阵列，需要用多个wire bonding以避免高电流熔断金线。如下图所示，左边是单一wire bonding，右边是多个wire bonding。根据Vixar，对于1 mil粗细的金线，最大可以承受0.7安培的电流。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-15.png" style="zoom:50%;" />

# VCSEL关键参数

VCSEL作为一种半导体激光器元件，有很多参数，其中关键参数为光学，电学和热学特性等。波长特性，包括波长，光谱宽度，波长和温度的关系；光束质量（模式，发散角）；输出功率和转换效率等。

## 波长特性（波长，光谱宽度，温度特性）

VCSEL的光谱特性由其量子阱中增益介质和DBR的设计决定。常见的VCSEL波长为650nm-1080nm，1550nm等，以红外应用为主，最新的研究可以实现280-320 nm的紫外VCSEL。对于有多个发光点的VCSEL阵列，由于制程误差，每个VCSEL的波长会略有偏移，整体展示出一个相对较宽的整体光谱，单个VCSEL Mesa的光谱宽度一般小于1nm。VCSEL波长对于温度十分稳定，为~0.06nm/K的数量级。

## 光束质量（模式，发散角）

VCSEL中的氧化限制层在DBR中制造了一个波导结构，VCSEL的横模类似于光纤中的弱波导模式。对于足够小的氧化限制孔径，只有基础模式LP01可以存在，扩大孔径会让更多的模式射出。VCSEL整体发出的光是由多个模式组成的，每个模式都有自己的发散角。基础模式LP01在远场是高斯光束，高阶模式发散角更大，光束质量也更差。下图（a）展示了不同模式的形态，图（b）展示了单一Mesa对于不同孔径的光束发散角的区别。VCSEL的光束发散角一般定义为包含一半Peak能量的发散角宽度。同时电流也会影响光束质量，图（c）（d）展示了电流是如何影响光束偏离理想高斯光束的，其中θm是测量的光束发散角，θ0是理想高斯光束的发散角。电流越低，光束质量越好，原因是大电流直接流出了限制孔径范围，造成更复杂的发光情况。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-16.png" style="zoom:40%;" />

## 输出功率和转换效率

根据应用需求的不同，VCSEL功率从mW到W级别。工作电流和孔径的大小都影响其输出功率。功率会随着电流增大而增大，达到一个峰值然后下降，如下左图所示，展示了直流情况下电流和功率的关系。这种现象的原因是半导体结内的温度上升导致的，被称为“thermal rollover”。可以通过使用脉冲模式来实现更高的峰值功率，如下右图所示，展示了1%duty cyle， 50ns pulse width，不同温度下的峰值功率。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-17.png" style="zoom:30%;" />

一般来说，单独的Mesa用大孔径会实现更大的功率。小孔径的VCSEL较小的电流密度限制了一个VCSEL可以实现的最大Wall Plug Efficiency (WPE)。但是由于VCSEL孔径的大小和DBR厚度比例的限制，出现最大WPE的孔径为某个值，然后WPE开始下降。假设这个值为8um左右，随着aperture变大，其中的电流分布就开始不均匀，影响了其WPE。为了实现高功率，如果要做一个VCSEL阵列，可能会选择使用10-12um的孔径，并制造成阵列。这种方式很好的平衡了最大的功率需求和电光转换效率，以及光束质量。下图展示了VCSEL的电光转换效率，孔径和温度的关系。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-18.png" style="zoom:40%;" />

VCSEL设计中还有很多其他需要考量的因素，包括其阈值电流，电阻，上升沿下降沿时间，工作温度范围，效率等。可以看到很多参数之间是相互影响，需要在设计中进行取舍和平衡。

## 信赖性

由于VCSEL是一种千层饼式的激光结构，这种结构的每层都是不同物质，同时每层都很薄，所以这种结构很容易收到各种影响，其中包括水汽，应力和热等。绝缘保护制程可以很好的增强防水氧侵袭，良好的封装散热也很重要，制造的参数控制也对应力很有帮助（比如会有退火的流程）。

# VCSEL企业和市场

## VCSEL企业

现阶段最大的VCSEL器件玩家主要是Lumentum，II-VI，Trumpf，Vixar等供应商，这些公司在VCSEL主要使用在通信领域的时期积累了非常多的经验，尤其是高信赖性的产品上有着独一无二的优势。在传感器领域，这些公司依然是主要玩家。国内的重要玩家是睿熙科技，纵慧激光和长光华芯等，都是在苹果发布FaceID时期，创办或开始重点研发VCSEL的企业。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-19.png" style="zoom:40%;" />

## VCSEL市场

根据Yole的调研报告（见下图）：

- **市场预期**

全球VCSEL市场预期在2026年增长到24亿美元的规模，21-26年CAGR为13.6%，主要驱动力是数据通信和手机应用需求。其中手机和电子消费品领域VCSEL规模预期达到17亿美元，21-26年CACG为16.4%；车用VCSEL在2026年预期达到5700万美元规模，CAGR 21-26 122%。

- **技术预期**

多结VCSEL(Multi-junction VCSEL)技术代表着VCSEL工业界的下一个大飞跃；同时VCSEL生产厂商从4"晶圆向6"晶圆升级，并很快会进入8"晶圆生产；对于OLED屏下传感的应用，由于组装方式和集成方式不同，可能会打破传统的供应链形式。

- **供应链**

Lumentum和II-VI掌握着80%的市场占有率，VCSEL的供应商很多，但是头部玩家很少，主要是中小型企业。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TOFSystem/C5/C5-20.png)

# 参考

1. [VCSELs - Fundamentals, Technology and Applications of Vertical-Cavity Surface-Emitting Lasers](https://link.springer.com/book/10.1007/978-3-642-24986-0)
1. [Vixar公司的一系列 Application Notes](http://vixarinc.com/vcsel-technology/application-notes)
1. [飞芯电子 一文读懂VCSEL](http://www.abax-sensing.com/newsInfo-10-9.html)
4. [Lumentum  VCSEL](https://www.lumentum.com/en/diode-lasers/products/vcsels)
4. [II-VI VCSEL](https://ii-vi.com/vcsel-technology/)
4. [Yole VCSEL Market Research](http://www.yole.fr/VCSEL_Market_Update_2021.aspx)

-----

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



