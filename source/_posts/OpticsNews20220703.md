---
title: 光学新闻 20220703
urlname: OpticsNews20220703
date: 2022-07-03 22:00:00
tag:  [光学新闻]
categories: 光学新闻
photo: https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/5.jpg
---

**近两周的五篇光学新闻：**

1.  ADI发布3D景深测量高分辨率iToF模块ADTF3175 --- 
2.  超表面的拜耳型分光滤色器---
3.  II-VI收购Coherent获得最终批准---
4.  Intel集成光子学研究最新进展--- 
5.  生产自太空的光学KDP晶体获得第一笔销售

<!--more-->

-----
## ADI发布3D景深测量高分辨率iToF模块ADTF3175

评论：ADI日前发布了首款用于3D景深测量和视觉系统的高分辨率、工业品质、间接飞行时间（iToF）模块：ADTF3175。参数如下：

- 1024 × 1024 TOF 传感器， 3.5 μm × 3.5 μm 像素大小
- 75 × 75 FOV
- 940nm 工作波段
- 深度探测工作范围0.4m to 4m (深度噪音最大15mm, 最小15%反射, <5 klux 等效阳光)

- 深度分辨准确度: ± 3 mm

值得一提的是，为了支持各种照明条件，这款高分辨率模块采用了Lumentum先进的三结垂直腔面发射激光器（VCSEL）技术，三结VCSEL可以增强发射端的发射强度。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/1.png)

[官网产品页](https://www.analog.com/en/products/adtf3175.html)

-----
## 超表面的拜耳型分光滤色器

评论：南京大学和华为技术有限公司共同开发了一种基于超构表面的像素级拜耳型分光滤色器（metasurface-based Bayer-type colour router，MBCR），该分光滤色器对红光、绿光和蓝光的峰值颜色收集效率分别为58%、59%和49%，并且在可见光区域（400nm-700nm）的平均能量利用效率高达84%，是商用拜耳彩色滤光片的两倍。此外，通过200µm × 200µm的基于超构表面的分光滤色器和单色成像传感器的一起工作，进一步实现了彩色成像，获得的图像强度是商用拜耳彩色滤光片的两倍。这项研究开创性地提出了一种高效的光谱信息获取机制，有望在下一代成像系统的开发中显示其广阔的应用前景。

下图展示了BCR和MBCR的概念图

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/2-1.webp" style="zoom:33%;" />

下图展示了研究人员通过任意初始化生成的设计，其是垂直方向上超构表面的一个单位单元，对应于四个像素。MBCR的单元周期为2µm × 2µm，与商用CMOS图像传感器的常用像素大小（1µm × 1µm）相匹配。由于RGGB图案具有对角对称性，且MBCR必须与偏振无关，因此在设计中必须将单元设置为对角对称。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/2-2.webp" style="zoom:33%;" />

为了对MBCR的分光滤色性能进行可视化，研究人员使用彩色苹果的图片演示了彩色成像，并将成像结果与商用BCF的成像结果进行了比较。MBCR和单色成像传感器均直接捕获带有马赛克图案的原始灰度图片。之后，MBCR的光谱响应被施加到每个像素上，使用转换矩阵方法获得带有马赛克图案的彩色图片。转换矩阵将检测到的光谱强度直接转换为三通道RGB值。最后将带有马赛克图案的彩色图片还原。即根据上述RGGB信息，进行像素插值算法（例如，去马赛克插值）以获得每个RGGB单元的RGB值。通过将RGGB像素值转换为一个三通道像素值，模拟实际彩色成像传感器中的三通道成像，最终重建彩色图像。结果同时表明，与商用BCF相比，通过MBCR获得的图像具有更高的亮度。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/2-3.webp" style="zoom:33%;" />



[论文](https://www.nature.com/articles/s41467-022-31019-7)

-----
## II-VI收购Coherent获得最终批准

评论：II-VI和Coherent终于要完成合并了，中国国家市场监督管理总局发布公告称，决定附加限制性条件批准II-VI收购Coherent。去年3月份，II-VI在和MKS及Lumentum的竞标中获胜，计划2021年收购Coherent。根据合并协议中的条款，在交易完成后，Coherent的每股普通股将兑换为220美元现金和0.91股II-VI普通股, 收购总价大约为70亿美元。此前，II-VI预计其对Coherent的并购将于2022年7月1日左右完成。根据官方介绍，本次并购完成后，II-VI和Coherent将共同创建一个全球领先的光子解决方案、化合物半导体、激光技术和系统公司，II-VI和Coherent在子系统和系统层面的激光光学和电子技术互补，将提供引人注目的解决方案，以加速航空航天和国防、生命科学和激光增材制造领域的增长。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/3.png)

[新闻稿](https://www.coherent.com/news/press-releases/ii-vi-completes-acquisition-of-coherent)

-----
## Intel集成光子学研究最新进展

评论：Intel Labs发布了“集成光子学研究的重要成果-增加计算芯片的通信带宽的下一代技术”。Intel最新的研究包括开发集成在硅片中的多波长分布反馈激光阵列，可以提供+/- 0.25 dB的输出功率均一性和±6.5%波长间距均一性。这个研究证明了，实现均一的输出功率和均一的间隔波长是可能的。更重要的是，这可以使用现阶段的制程实现，因此保证了下一代的光学传输和计算整体封装的可能性。Intel相信该成就可以用于未来的高速通信应用。市场分析机构Gartner预测2025年硅光子学技术会使用在20%的高带宽数据中心中（2020年<5%），总计26亿美元的市场。硅光子技术可以提供数据中心需要低能耗，高带宽，快速传输数据能力。下图展示了8通道的激光阵列

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/4.jpg)



[新闻稿](https://optics.org/news/13/6/48)

-----
## 生产自太空的光学KDP晶体获得第一笔销售

评论：Redwire公司宣称其完成了一笔销售，将在太空中生产的2克potassium dihydrogen phosphate (KDP)晶体卖给了俄亥俄州立大学的电子显微镜和分析实验室。根据成交价格，该批物质达到了200万美元每公斤的价格。该批物质是由Redwire公司的Industrial Crystallization Facility生产的，该设备被放置在国际空间站。这笔交易是第一次太空中生产的材料在地球上被销售，这是太空商业化的一个重要里程碑。Redwire声称太空生产的光学晶体可以极大地提升高能激光的表现，现阶段在地球上生产的光学晶体由于受到重力影响，会产生杂质或缺陷，最终限制了高能激光的输出功率，而太空中生产的晶体不会。俄亥俄州立大学的研究者们会比较地球和太空中生产的晶体的区别。

考虑到只卖了2克，其实只有$4000，:p.不过太空生产，可能会越来越多吧。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20220703/5.jpg)

[新闻稿](https://optics.org/news/13/6/44)

-----

这里记录值得分享的光学科技内容，欢迎投稿或推荐科技内容。

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



