---
title: 光学新闻 20210911
urlname: OpticsNews20210911
date: 2021-09-11 16:30:00
tag:  [光学新闻]
categories: 光学新闻
photo:  https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/5.png
---

**本周五篇光学新闻**：

1.  Sony基于事件的图像传感器 --- 
2.  Sony堆叠式SPAD深度传感器 ---
3.  Vivo首颗自研专业影像芯片  ---
4.  光粒发布AR全息智能泳镜Holoswim --- 
5.  OQmented单芯片MEMS激光束扫描解决方案

<!--more-->

-----
## Sony联合Prophesee开发两款基于事件的图像传感器

评论：Sony发布了两款堆叠式，基于事件的图像传感器，这一系列传感器是和西班牙初创公司Prophesee合作开发的。一款为IMX636 1/2.5" 0.92MP，一款为IMX637 1/4.5" 0.33MP。基于事件的图像传感器通过异步检测每个像素的光强变化，只输出变化的数据，结合像素点的位置信息，可以实现高速低延迟的数据输出。下图展示了普通基于帧的图像传感器和基于事件的图像传感器的区别，基于帧的图像传感器需要输出每一帧的所有像素内容，而基于事件的图像传感器只输出变化的像素点的信息，包括位置，时间等，大大降低了输出数据的量。由此基于事件的传感器可以实现高速，高时域分辨率的传感。该种传感器有着非常多的应用场景，包括低光照条件下的运动检测（夜晚道路监控，没有人或者车的时候，并没有大量信息输出，当场景出现运动物体时，才有输出），工业设备监控（机床监控，只输出运动部分的信息），对于电子消费品，VR头盔的SLAM，手柄追踪，眼球追踪可能未来也可以使用这种传感器，不过现阶段该种传感器像素比较低，可能手柄和眼球追踪可以先实现。

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/1.png" style="zoom:67%;" />

[新闻稿](https://www.sony-semicon.co.jp/e/news/2021/2021090901.html)

-----
## Sony发布业界首款面向汽车dToF激光雷达应用的堆叠式SPAD深度传感器

评论：Sony发布了IMX459，首款面向激光雷达应用的SPAD深度传感器，该款传感器由10um大小的像素点，1/2.9"大小，0.1MP，使用背照式，堆叠式，索尼的Cu-Cu连接技术，在905nm处可以获得24%的光子检测效率，可以实现在短距离到长距离15cm的深度传感分辨率。该款传感器通过了AEC-Q100 Grade 2自动驾驶电子零部件的信赖性测试。基于该款芯片，索尼还开发了一个机械旋转式的激光雷达参考设计。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/2.png)

[新闻稿](https://www.sony-semicon.co.jp/e/news/2021/2021090601.html)

-----
## Vivo发布首颗自研专业影像芯片

评论：Vivo本周发布了vivo X70系列，这次最值得关注的点就是Vivo首次使用了自己的自研专业影响芯片V1。其中X70 Pro+，主摄使用了三星的5000万GN1传感器，1G+6P镜片；超广角使用了Sony的4800万像素的IMX598；人像镜头使用了Sony的IMX663。这次的V1芯片是由vivo与手机SoC厂商深度合作（行业消息称是与联咏合作设计，台积电投产），历时24个月、投入超300人研发而来。通过影像芯片V1与主芯片协作，可以实现高算力、低时延、低功耗的特性。看了一些评测视频，V1芯片对于夜景的拍摄，实时预览，录像防抖等都有很大提升。一般芯片开发周期是两年左右，一旦一家公司的自研芯片路线开始，就会一直走下去，所以我们明年应该可以看到V2芯片。对于国内手机影像方案，自研芯片是差异化竞争及走上高端手机产品线的必经之路。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/3.png)

[发布会视频](https://www.bilibili.com/blackboard/activity-ZTXKZ4stR9.html?spm_id_from=333.337.0.0)

-----
## 光粒发布AR全息智能泳镜Holoswim

评论：AR智能眼镜公司[杭州光粒科技](http://www.lightin.com)发布了新品——Holoswim™全息智能泳镜。该款产品已经登陆Kickstarter开启众筹，众筹价格为99美元。相同产品有加拿大公司[Form AR](https://www.formswim.com)的产品，不过售价要199美元，这次光粒的价格99美元可能在欧美市场会比较受欢迎。泳镜AR还有一些运动类护目镜AR一直是一个小众赛道，比如自行车，滑雪等。在这些产品里实现AR并不困难，如何让消费者买单是重要的课题。如果可以结合类似GoPro的视频录制功能，可能会更有销路。下面两张图里，上面是光粒科技的产品Holoswim，下面是Form公司的产品。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/4-1.png)

<img src="https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/4-2.png" style="zoom:50%;" />

[Kickstarter链接](https://www.kickstarter.com/projects/holoswim/holoswim-your-smart-ar-swimming-goggles/faqs)

-----
## OQmented推出业界首款基于单芯片MEMS微镜的激光束扫描（LBS）解决方案

评论：MEMS微镜技术初创公司[OQmented](https://oqmented.com)，近日宣布推出业界首款基于单芯片MEMS微镜的激光束扫描（LBS）解决方案。OQmented这款高速LBS解决方案采用晶圆级微型化真空封装，使用了OQmented的Bubble MEMS技术。这种封装形式将MEMS封装在一个真空的半球形盖子中，可以很好滴保证大角度的FOV的扫描（平面封装盖板会造成光损失或者造成整体模组变大，而不用盖板封装可能会造成一些信赖性问题。OQmented成立于2018年，其解决方案的始于德国弗劳恩霍夫研究所。这次的解决方案采用了专有的李萨如扫描（Lissajous scanning）技术，并专门针对AR/VR应用进行了优化。该公司是LaSAR Alliance (Laser Scanning for Augmented Reality)，AR激光扫描联盟的一员，该联盟致力于进行AR相关的制造合作，包括多家企业，其中有：ST，OSRAM，Dispelix，Applied Material等。

![](https://cdn.jsdelivr.net/gh/Sterncat/BlogPics/OpticsNews/20210911/5.png)

[新闻稿](https://www.globenewswire.com/news-release/2021/09/08/2293019/0/en/OQmented-Introduces-Industry-s-First-One-Chip-MEMS-Mirror-Based-Laser-Beam-Scanning-Solution-to-Enable-AR-VR-Smart-Glasses.html)

-----

这里记录值得分享的光学科技内容，欢迎投稿或推荐科技内容。

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



