---
title: Sony Xperia 1&5 Mark III变焦镜头
date: 2021-05-16 00:05:00
tag:  [镜头, 技术评论]
categories: 技术评论
photos: https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/1.png
---

Sony 近期推出了自己的Xperia 1和Xperia 5 Mark III手机，里面很有意思的是一款变焦镜头。说变焦可能不太准确，因为这款镜头并是不连续变焦镜头，而是一款只实现两个焦段70mm和105mm的潜望式镜头，不过确实焦距变了，叫变焦也无可厚非。所有70mm~105mm中间的焦距都靠数字变焦实现。这次的三颗摄影镜头的参数如下。

<!--more-->

![](https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/2.jpg)

| 等效焦距  | F/#        | 图像传感器    | 图像传感器大小 |
| :-------- | :--------- | :------------ | -------------- |
| 16mm      | F2.2       | 12MP, Dual-PD | 1/2.6"         |
| 24mm      | F1.7       | 12MP, Dual-PD | 1/1.7"         |
| 70, 105mm | F2.3, F2.8 | 12MP, Dual-PD | 1/2.9"         |

通过索尼的视频宣传可以看到，实现这两个焦段主要是靠三组镜头，通过移动后两组来实现。

![](https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/3.gif)

虽然以前有几款手机实现了连续变焦，有很多手机型号实现潜望式，但是索尼是第一次采用这种折中的方式：在潜望式中实现部分的变焦功能。连续变焦模组应用在手机里的有Asus Zenfone Zoom，是用了Hoya的[Hoya CUBE lens unit](https://www.hoya.co.jp/english/company/news/20130801.html) (2013)，该模组是实现28mm-84mm的等效焦距的连续变焦（F/2.7-F/4.8)，但是看真机图也能看出来这个模组还是很占空间的。索尼这次提供的70mm-105mm(F/2.3-F2.8)，看比例，可能索尼在甚至更小的模组空间里实现了比Hoya模组更高的光学性能（两者芯片大小几乎相同）。从产品角度考虑，普通消费者确实不是很关心是否是连续变焦，甚至不关心是否可以变焦，单一焦段的潜望式普通消费者用着也很好。这次采用这种方案更像是Sony的一次创新性尝试和市场卖点。不过模组的实现上，还是有很多有趣的地方的。

<img src="https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/4.jpg" style="zoom:50%;" />

![](https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/5.jpg)

Sony说该项技术的难点是让驱动器（accurator）实现高精度的镜组移动和相对位置。Sony说理论上他们可以实现连续变焦，只是在这款产品上选择不去使用。这句话可能指的是光学上可以，只需要更改机械结构来实现连续变焦。也可能指的是他们的机械结构已经有了这个能力，但是考虑到对焦精度，速度，信赖性，算法等多方面问题，决定不使用。既然Sony提到了Accurator，可能指的是第二种意思，那就意味着Sony可能是使用了类似Hoya模组内镜组的移动方式。[参考Hoya专利](https://patentimages.storage.googleapis.com/31/05/13/42943b59e3e33a/JP2015079047A.pdf) , Hoya使用的是两个滑竿的结构（见下图）。

* 这种变焦模组的生产方式在镜头厂制造方式：

第一种可能是镜头厂是做出三组镜头然后单独测试，最后在模组厂进行组装。这种方式镜头生产相对容易。

第二种可能是在镜头厂是三组镜片的搭配好，出货时是整组出货。这种方式测试如何进行是个问题。

* 在模组厂的制造方式：

组装后需要通过校准算法确认镜组在不同焦段的位置，写入模组的驱动器。这就需要第一个可移动镜组移动到一个位置，然后第二个可移动镜组移动到图像最清晰位置，记录位置，然后重复此过程。如果每1mm焦距一个移动位置记录，就需要35次测试。在实际应用中，可能模组还会进行对焦后的微调来补偿驱动器的漂移等问题。

这样生产中最终的生产效率可能不会很高，但是考虑到Sony这两款产品的实际出货量，就算该模组UPH低一些贵一些也没什么问题😂。可能产品端还是考虑的对焦速度和准确性以及图像质量问题，没有选择使用连续对焦。很有意思的是，在宣传视频中，出现变焦这部分下面一行小字“The Feather require software update, roll out time may vary by market and/or opertor”，看起来连续变焦的特性等软件更新可以实现。那这么看，更多的是软件上和校准上的问题导致现在只有两个焦段。

这些现在也都只是猜测，更具体的信息估计要等实际拆解后可能能看出端倪。以后有更多信息后再去更新该文章。

![](https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/6.jpg)

![](https://raw.githubusercontent.com/Sterncat/BlogPics/main/TechReview/1-SonyZoom/7.png)





-----

版权声明: 感谢您的阅读，本文由[超光](https://faster-than-light.net/)版权所有。如若转载，请注明出处。



