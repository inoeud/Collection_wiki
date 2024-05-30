> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/3EhWxpAzPX0NnI_JpwOgvQ)

  

(1) 什么是 DAC？

DAC，全称是 digital-to-analog converter，即是数模转换器，一般主要用在发射链路。

而 ADC，全称是 analog-to-digital converter，即模数转换器，一般用在接收链路。

这两个，都是模拟和数字的分界面，DAC 是把数字变成模拟信号，ADC 是把模拟信号变成数字信号。

(2) 先来看看实际 ADC 的采样过程 ---sample and hold

在[使用 ADC 之前，先来了解一下采样定理](http://mp.weixin.qq.com/s?__biz=MzU0MTAzMjkyMg==&mid=2247496179&idx=1&sn=3dc35f5c84b05d656210807cd089f376&chksm=fb329946cc451050a6e87f875a08c322de897a78ed435955535a96d5c2cca8de7406d2554ff2&scene=21#wechat_redirect)，讲的采样定理，是理想采样。

但是实际上，ADC 常用的采样方法是 sample and hold，也就是说，在采样点得到一个值，然后在采样周期内保持不变，直到下一个采样点更新。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/9JDxAJNCQyfoVtYG9Mznh4oMziaUQp0OFh2uL9yYGjmaTVvByfVl0vmWR9n83KSaxzSKwWjOZAumj66CN0icwsBQ/640?wx_fmt=png&from=appmsg)

也许你会说，你题目中讲的不是 DAC 么，怎么讲到 ADC 的采样了。

那是因为，在 ADC 中的 sample and hold，和 DAC 中的 zero order hold 是等效的。虽然名字不一样，但是出来的波形都是一样的，都是阶梯状的波形，如上图所示。

前面讲过，理想采样后，会得到冲激脉冲序列，如下图所示。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/9JDxAJNCQyfoVtYG9Mznh4oMziaUQp0OFGQqWJsuT0aUdnb0LXkDCBx8pibcBB6fb2XrMZMZSLhWr6cfTGMTEX6g/640?wx_fmt=png&from=appmsg)

将这个冲激脉冲序列与宽度为 Ts 的矩形脉冲进行卷积，即可得到实际 ADC 经过 sample and hold 后得到的波形。虽然从频谱上来看，两者相乘后，会有一点点失真，不过，我想，这种固定的失真，后面的算法应该能补回来的吧。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/9JDxAJNCQyfoVtYG9Mznh4oMziaUQp0OFFiahUCxjGWc6jFia6EVbnrucXS7soTNUlv9VTFsrKpjZrkPzpP7T6e8w/640?wx_fmt=png&from=appmsg)

(3) 为啥说，在用 DAC 之前，也还是要了解一下采样定理呢?

一般 DAC 工作时，其实和上图中演示的 ADC 的 sample and hold 的采样过程一样，把输入的数字信号的值，在一个周期内保持不变，直到下一个采样点更新。这个步骤，在 DAC 中，称为 zero order hold。

所以，如上面分析的频谱所示，当信号变成模拟信号后，除了我们需要的频谱以外，在其他频率处也有信号。但是由于矩形脉冲带来的 sinc 频谱，所以幅度会有所抑制。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/9JDxAJNCQyfoVtYG9Mznh4oMziaUQp0OFb0J0ezBn25wgv0sZwKe3Xg7E7ZI0J7wdN7UWPKMwELM86mQxRlD5SQ/640?wx_fmt=png&from=appmsg)

参考文献：

[1] http://www.dspguide.com/CH3.PDF

[2]  https://www.ti.com/lit/an/slaa523a/slaa523a.pdf