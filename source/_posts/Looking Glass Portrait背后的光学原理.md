---
title: Looking GLass Portrait背后的光学原理
urlname: TechReview_2_LookingGlass
date: 2021-05-21 02:00:00
tag:  技术评论
categories: 技术评论
photos: https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-1.jpg
---

去年12月[Looking Glass Factory Inc.](https://lookingglassfactory.com)在Kickstarter上进行众筹，[Looking Glass Portrait项目](https://www.kickstarter.com/projects/lookingglass/looking-glass-portrait)，全息显示相框产品，众筹早鸟价格199美元，虽然中间由于疫情导致产品所使用的Raspberry Pi缺货等原因延迟了一些，该产品5月第一批还是顺利发货。产品收到后第一时间点亮还是很惊艳的，看着三维的航天员在相框中缓慢的跑动，机械狗等demo，有一种赛博朋克2077的感觉，或者猫猫和狗狗的demo也很讨人喜欢。该款产品的宣传也可以直接通过公司官网的[Portrait产品页面](https://lookingglassfactory.com/portrait)观看。这个小相框摆在桌面上，很容易让自己或者路过的人多看两眼。作为该公司第一款To C的电子消费品（该公司已有两款前期产品，Looking Glass 15.6"和Looking Glass 8K，都是相对更大的产品，光学方案可能不同），笔者认为该产品打磨的很好，产品交互很简单，内容制作相对容易。这篇文章主要只探索Looking Glass Portrait背后的光学方案。来看一下它是如何显示3D信息的。

<!--more-->

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-2.jpg" style="zoom:50%;" />



我们先看看Portrait的性能参数和它能显示何种3D

## 技术参数

**Viewing cone可视角度：**58°

**No. of views可视的三维信息片**：45-100

**Input resolution输入分辨率**：1536像素 x 2048像素

**Aspect ratio长宽深比：**4:3:2

**显示屏：**7.9”/20cm对角

**显示屏：**Advanced high precision lens and microlouvre array 高精密镜片和微百叶窗阵列(我们马上就会讨论这到底在说什么)

## 显示效果

1. 不同角度看过去显示是不同的。
2. 人的双眼看，是有深度信息的（视差）。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-3.jpg)

## 光学原理

### 视差显示

再加上一些细节观察，结合这些技术参数我们可以逐步破解Looking Glass Portrait背后的光学原理。打了一束光到设备上，发现反射光出现了一道条纹，角度一直是如图显示的，略微倾斜。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-4.jpg" style="zoom:33%;" />

近距离观看，发现反光的条纹是一条条的结构自上而下有一个角度穿过整个显示屏。再结合技术参数里说的Advanced high precision lens and microlouvre array 高精密精品和微百叶窗阵列，第一个猜测就是这是用了柱镜光栅的原理（Lenticular lens）

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-5.JPG" style="zoom:50%;" />

对于柱镜光栅，我们其实都在生活中见过，就是常见的3D卡片。

![R2-6](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-6.gif)

柱透镜立体光栅实现立体成像原自于视差立体法，下图展示了两种视差立体法，一种是通过视差遮挡，一种是通过柱镜。视差遮挡法是用在显示图像前加上规律变化的遮挡条纹，让观察者在某个距离范围内，左眼只能看到一部分图像，右眼只能看到另一部分图像，这样就可以把有视差的两张图像分割成两组条纹图像（R和L）。柱镜立体法原理类似，形态如图所示。

<img src="https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/2-LookingGlass/R2-7.png" style="zoom:50%;" />

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-8.jpg)

下一个问题就很有趣了，Looking Glass Portrait是如何实现这么丝滑的立体显示的（我们小时候用的卡片图像变化都很快，然后也没有丝滑的连续的变化感觉）？为什么这个设备里的柱镜是倾斜的？

通过查询Looking Glass Portrait的几个专利，发现在[US20190268588A1](https://patents.google.com/patent/US20190268588A1/en?assignee=Looking+Glass+Factory%2c+Inc.&num=50&sort=new)里提到了使用Lenticular lens等方法产生视差，同时专利里提到了另一个专利描述了图像分割来实现期待的显示效果.（This technique is described further in [U.S.Pat.No.6,064,424](https://patentimages.storage.googleapis.com/37/e6/27/c427a4c1441125/US6064424.pdf). Images licing or division(of light source 10 output) may be accomplished in any manner to achieve a desired viewing result.）。找到另一个专利，发现是飞利浦公司1997年的专利，主要描述了如何充分使用电子显示屏幕的相同的横向和纵向分辨率和柱镜结合来完成3D显示（该专利已于2017年过了专利保护期）。

我们先讨论柱镜3D显示的一个问题，是如果柱镜是竖直的，我们水平左右移动从不同不同角度就可以看到不同的视角（view），如果我们想看到很多视角，我们就可以横向去在一个柱镜里去添加更多的图像块（视角），如下图所示，展示了4个view如何构建，如果我们需要在在4度的观察视场角里，丝滑的观察图像，我们需要在一个柱镜的范围里添加4个view，让我们每一度里都有一个不同的角度的图像信息。如果我们的显示部分横向纵向分辨率是一样的，我们会发现，由于柱镜的存在，我们的横向分辨率比纵向分辨率低4倍。这个问题如果是使用打印的图像，很好解决，我们纵向上只需要降低4倍打印分辨率，这样我们横向和纵向分辨率就是1：1了。但如果我们用的是一个电子显示屏，我们如果这么做去把横向纵向分辨率调节成一样，我们就要不去显示纵向的一些像素点，那就拜拜损失掉了我们显示屏纵向的像素点的作用。当我们想在50度视场角里丝滑的显示50个view时，我们需要在一个柱镜里并列塞进去50个图像slice（view），这就出现了两个问题，第一个问题是我们刚才提到的（1：50的分辨率区别太大了，我们浪费掉了98%的像素），第二个问题是我们的柱镜可能要做的很宽，去塞下这些图像slice，对于一个300 PPI的显示屏，一个像素点（RGB）是85um，50个就是4.25mm，已经肉眼的一定距离可见柱镜了。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-10.jpg" style="zoom:40%;" />

飞利浦提出的专利通过倾整个柱镜的面，让其和显示屏呈现一定角度，来利用其纵向的显示屏像素点。如专利的下图所示，16是柱镜，条状块是像素点。通过让柱镜面和显示屏呈现一个夹角，可以看到view 1最靠近柱镜左侧，然后依次是2，3，4，5，6。这样，每两排像素点会构成了一组全信息。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-11.png" style="zoom:80%;" />

回到Looking Glass Potriat，产品信息里，输入分辨率是1536像素 x 2048像素，No. of Views是45-100，我们可以实际数一下到底有多少个柱镜。通过下图照片，简单数了一下，是大概横向250个左右，考虑横向像素点一共1536个，我们可以猜测一共大概有256个柱镜，正好是6倍的关系。那这样，横向每个柱镜就可以放入6个像素点，如果要达到产品信息的No. of Views的最低要求45，纵向我们需要8~16行像素。8应该是最合理的，因为这样还基本在一个正方向的空间内显示完整的48个view的信息。宏观分辨率是256 x 256（1536/6 = 256, 2048/8=256）。大概计算了一下产品中柱镜的角度，是8°左右，如果是柱镜的一侧边从上到下通过8个像素，从左到右通过1个像素，那么是7.12°。不过这只是大概观察，主要是为了说明原理。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-12.png)



我们尝试画一个图展示如何显示这48个像素点，每个像素点都在显示什么。Looking Glass Portrait的柱镜是向左倾斜，但是为了结合专利图示，我们的示意图按专利图示也向右倾斜。最终画出来的图如下图所示，我们想象成每一个黑色框中是一个空间信息pixel（我们叫它大pixel），每一个这样的pixel结合柱镜可以展示48个角度的图像信息。这样，Look Glass Portrait就成为了一个256 x 256像素的空间信息显示屏。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-13.png" style="zoom:60%;" />

### 颜色显示

如何处理颜色呢？我们知道每一个小像素点都是由RGB像素点组成的，可以显示各种颜色，但是如果简单的正常显示，对于1-48这些角度的信息，由于他们和柱镜的相对位置不同，造成显示时，亮度可能会有所不同（出射光角度，杂光等问题），同时由于不同角度的放大率不同，会造成色彩条纹的问题。飞利浦的专利提出了几种方式去解决色彩条纹的问题，而且我们可以从Looking Glass Portrait的显示中看到应用了类似的方法。飞利浦展示了一个7 view的系统，我们把每7个view叫做一个大像素。

第一种方法是使用第一个大像素的view 1显示红，第二个大像素的view 1显示绿，第三个大像素的view 1显示蓝，4A展示了这个概念，4B展示了大像素层面，rgb是如何现实的。这个方式的问题是彩色条纹还是会出现，会出现在对角（图4B，r练成了一条线）。所以飞利浦又提出了两种其他的方法去解决这个问题

方法二和方法三的概念相同，比如方法二，用第一个大像素的第一行所有view显示红，然后右下角的大像素的第一行所有view显示绿，然后这个大像素的再左下角的大像素的第一行所有view显示蓝，见图5A。在5B中，虚线类似梯形框起来的就是一个大号的色彩pixel，这种显示问题是会和下一个显示点的信息有cross talk，因为在这个虚线框里会有一个来自于另一个大号色彩pixel的g'。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-14.png)

我们看看Looking Glass Portrait是如何排布颜色的，通过放大镜拍摄像素点，这是一片宏观上（远看）显示为白色的区域，我们发现像素颜色是如图所示的，我们可以大胆假设一下可能的方式。竖向的倾斜线是柱镜的边界，可以看到纵向颜色的重复正好是8行，如果继续放大是可以看到方形的显示屏基础像素点的。观察这个像素点我们发现可能1-48view和我们的简单排布可能不太一样，看起来是个类似菱形，可能这些行之间有shift以构建完整的256x256pixel，让算法更方便（矩形排布在边角会有半个大pixel的情况）。然后就是颜色显示上，看起来每个大pixel上部分显示r，然后b，然后g的总体思路，但是实际上混色是一个比较复杂的组成而不是简单的某一行展示单色。个人认为可能这样做的原因应该是可以让显示效果更好，毕竟飞利浦提出专利后显示屏技术也发展了20年，而且Looking Glass Portrait的每个大像素里像素多得多，也小的多。如果有更好的放大设备，我们可以一探究竟。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/TechReview/2-LookingGlass/R2-15.png)

## 总结

上面的分析当然都是基于表面上看到的信息得出的，不过如果真的是如分析所说，Looking Glass Portrait并不是像产品介绍中一样使用了“next generation holographic display technology”（下一代的全息显示技术），相反，它使用了“last generation technology”（上一代的技术，飞利浦的专利是1997年的）。不过，作为一种产品宣传策略这可以理解。而且，这并不妨碍我认为这是一款好产品，无论是从显示效果，产品形态上，还是价格上，都可以说是一款85分以上的作品。

需要探索的问题还有很多，比如产品宣传里所说的45-100view，我们分析了48view的情况，如果100view，Looking Glass Portrait是如何实现的？而且这种连续的分view是否能在软件端实现？还是这种说法只是产品宣传？个人认为可能是为了避免技术上的猜测，实际的变化范围可能是48-96view。

对这个产品的分析过程有一种侦探探案的感觉，也确实是拿着一个放大镜还有开着闪光灯去仔细观察这个产品，格物，致知。

-----

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



