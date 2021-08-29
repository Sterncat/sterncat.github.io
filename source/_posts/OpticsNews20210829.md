---
title: 光学新闻 20210829
urlname: OpticsNews20210829
date: 2021-08-29 15:00:00
tag:  [光学新闻]
categories: 光学新闻
photo: https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/2.jpg
---

**本周五篇光学新闻**：

1.  Ouster就收购竞争对手Sense Photonics展开会谈 --- 
2.  Mojo Vision可嵌入隐形眼镜的微显示器原型Mojo Lens ---
3.  血液光谱仪展现了Covid-19分诊能力  ---
4.  相干TOF实现250um的深度分辨率 --- 
5.  dTOF实现26um深度分辨率

<!--more-->

-----
## Ouster就收购竞争对手Sense Photonics展开会谈

评论：激光雷达公司Ouster，在今年初上市后，开始了一些收购和并购行动，现阶段正在和竞争对手Sense Photonics进行谈判，讨论收购事宜，具体的收购价格未知，Ouster的发言人也拒绝评论，Sense Photonics的代表也没有回复问询。作为一家总部在旧金山的激光雷达公司，该公司开发的激光雷达可以提供每秒一千万点数量的点云，其他激光雷达一般每秒可以提供200万点的点云，更多的数据点可以让激光雷达不单追踪车道上的汽车，也帮助其追踪对于自动驾驶极其重要的外部的环境。Sense Photonics声称其公司技术由300份专利保护，同时公司的资金由多家风投公司支持，其中包括Acadia Woods, Congruent Ventures, Prelude Ventures, Samsung Ventures, Shell Ventures, Hemi Ventures，IPD Capital。Ouster和Sense Photonics的激光雷达技术非常类似，合并确实有助于减少竞争，强强联合，因为在激光雷达赛道，还有非常多的其他竞争对手如Velodyne等，在Flash Lidar赛道如果两家公司一直进行竞争有可能最终丧失更大的机会。下图（[链接](https://sensephotonics.com/learn/)）展示了Sense Photonics宣传中对于激光雷达的分类，其中第一代是机械旋转式，第二代是使用MEMS的半固态激光雷达，第三代是使用光学相控阵方式的固态激光雷达，第四代是Sense Photonics（以及Ouster）开发的Global Shutter Flash型激光雷达

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/1.gif" style="zoom:40%;" />

[新闻稿](https://www.bloomberg.com/news/articles/2021-08-19/ouster-is-said-in-talks-to-acquire-lidar-rival-sense-photonics)

-----
## Mojo Vision可嵌入隐形眼镜的微显示器原型Mojo Lens

评论：[Mojo Vision](https://www.mojo.vision)是一家致力于开发隐形眼镜形态的微型显示器的公司，该公司迄今为止获得了New Enterprise Associates、Liberty Global Ventures和Khosla Ventures等风投机构提供1.59亿美元融资。近期该公司发布了一些信息，展示了最新的进展。下图展示了Mojo Lens的结构，其中包括了环形的电路结构，其中直接包含了一个CMOS传感器，无线模块，电源权利模块，位移传感器等，中间的是关键的六边形显示模块，大小有0.5mm左右，每个像素点只有红细胞的四分之一大，最后设备使用“飞秒投影仪”，一个小型的放大系统，把影像投射到视网膜上。同时该设备还依赖一系列外部模块去控制这个隐形眼镜大小的模块。 Mojo Vision没有透露何时发货，也没有任何实际演示或实机视频。团队正在和日本隐形眼镜厂商Menicon合作进行开发。不过该技术的商用道路还有一段距离，不过可能可以先给弱视等视觉障碍人群提供一些可能性。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/2.jpg" style="zoom:50%;" />

[新闻稿](https://www.cnet.com/tech/mobile/mojo-vision-crams-its-contact-lens-with-ar-display-processor-and-wireless-tech/)

-----
## 血液光谱仪展现了Covid-19分诊能力

评论：印度和澳大利亚的研究者们的实验展示了使用红外光谱仪进行血液检测，可以作为一种简单快速地分诊手段，使诊所和医院更容易区分哪些Covid-19的感染者要可能有着更重的症状，需要更多的医疗关注。这个小规模实验使用的是安捷伦公司的傅里叶变换式红外光谱仪[FTIR](https://www.agilent.com/en/product/molecular-spectroscopy/ftir-spectroscopy/ftir-benchtop-systems/cary-630-ftir-spectrometer)，研究者们通过光谱仪区分轻症和重症患者血液样本的轻微光谱差异。这次实验使用了130个样本去训练算法，然后测试了30个样本。当结合其他参数如年龄或者其他基础疾病和高血压时，这种方法可以确定大部分可能会成为重症的患者。不过FTIR的分诊方法也产生了很多“假阳性”的结果，算法认为这些患者可能会进入重症，但是实际上并没有。研究者们认为测试结果的精准度可以进一步提升，帮助医院来优先处理可能的未来重症患者。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/3.jpg)

[新闻稿](https://optics.org/news/12/8/33)

-----
## 相干iTOF实现250um的深度分辨率

评论：加州理工大学的研究者们发表了一篇名为“IQ Photonic Receiver for Coherent Imaging with a Scalable Aperture，正交/同相光子接收器用于可变光圈相干成像”的论文。其中描述了商用iTOF使用光强检测的CCD或者CMOS，这种测量相位计算深度的方式由于噪声等问题有着受限的深度分辨率。使用硅光子（Silicon Photonics, SiP）集成相干图像传感器可以提供更高的灵敏度以及深度分辨率，前一代的SiP传感器受到接收信号和参考信号之间光学相位波动的影响，最终会造成输出信号的结果中有着一些随机想为何强度波动。这限制了系统的SNR和信号获取时间。在本篇论文中，研究者们提出了一种创新的相干传感器系统，使用正交/同相光子接收器，可以抑制光学载波信号并移除参考光路中的非理想因素。同时团队提出了这种传感器系统的行列读出系统和寻址系统的设计方案，可以在N*N的传感器阵列上只使用2N的连接。研究者们使用8 x 8的芯片阵列进行了概念验证，实现了1m距离上250um的深度分辨率。可以看到在学术界，对于深度传感的研究还在向着更高的精度进行，这些技术可能会在未来逐渐商用化。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/4-1.JPG" style="zoom:67%;" />

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/4-2.JPG" style="zoom:50%;" />

[论文](https://arxiv.org/abs/2108.10225)

-----
## dTOF实现26um的深度测量准确度

评论：科罗拉多大学波德分校的研究者们发表了一篇名为“Time-magnified photon counting with 550-fs resolution，550飞秒分辨率的时间放大光子计数”的论文，展示了进一步提升dTOF（直接飞行时间）深度识别技术深度分辨率的可能性。dTOF深度识别使用的技术被称为**时间相关单光子计数法**(TCSPC，Time correlated single photon counting)，但是这种方法的单光子时间分辨率被限制在3-100皮秒（转换成距离就是0.9mm-30mm的深度分辨率，这个限制来自于单光子探测器SPAD的极限）。在这篇论文中，研究者提出了一种被称为**时间放大时间相关单光子计数法**(TM-TCSPC，Time magnified-Time correlated single photon counting)的方法，实现了使用商用单光子探测器实现了单光子时间分辨率550飞秒，这种技术可以对一个130飞秒的脉冲实现22飞秒的准确度，当使用在TOF 3D成像中，该方法可以极大地提升系统准确度（提升99.2%），实现26um的准确度和3um的精密度。具体实现方式读下来应该是可以说让“光多跑一会儿，给传感器留出些时间”，使用的关键元件是量子时域放大器，结合一个基于四波混合布拉格散射的低噪声高效率的光线参数时间透镜，把时域放大130倍。相当于让传感器以更慢的频率接收到信号。参考上一篇文章，可以看到dTOF的研究也在快速的发展。随着集成光子学的发展，我们可能很快就可以看到使用类似方法的小型化设备。不过电子消费品领域暂时还没有对如此高的精度有需求。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210829/5.jpeg)

[论文](https://www.osapublishing.org/optica/fulltext.cfm?uri=optica-8-8-1109&id=457276)

-----

这里记录值得分享的光学科技内容，欢迎投稿或推荐科技内容。

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



