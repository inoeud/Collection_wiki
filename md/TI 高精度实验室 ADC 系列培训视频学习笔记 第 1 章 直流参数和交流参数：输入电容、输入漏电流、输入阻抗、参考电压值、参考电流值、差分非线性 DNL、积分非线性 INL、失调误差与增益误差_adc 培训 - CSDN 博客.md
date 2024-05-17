> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/keilzc/article/details/121723153?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3-121723153-blog-136063408.235%5Ev43%5Epc_blog_bottom_relevance_base2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3-121723153-blog-136063408.235%5Ev43%5Epc_blog_bottom_relevance_base2&utm_relevant_index=4)

[TI 高精度实验室 ADC 系列培训视频（B 站）](https://www.bilibili.com/video/BV1TJ411V7Ua?spm_id_from=333.999.0.0)  
[TI 高精度实验室 ADC 系列培训视频（21ic）](https://edu.21ic.com/video/2403)

### **第一章：直流参数和交流参数**

### **输入电容 寄生电容 采样电容**

当进行采样时 ，S1 开关闭合 ，采样电容与输入信号连接在一起到 ADC 的输入端，输入信号对采样电容进行充电，这段时间称为采样时间。  
在保持模式下 ，S1 开关断开，ADC 开始转换采样的信号，这一段时间称为转换时间。  
转换完毕后，S2 开关闭合，采样电容放电，为下一采样操作做准备。  
![](https://img-blog.csdnimg.cn/ea21f1310748403093d581901e3eae24.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
如右上图具体模型中，寄生电容 4pf 始终存在，采样时开关闭合，以采样电容 60pf 为主，保持时开关断开，只有寄生电容 4pf。

### **输入漏电流**

输入漏电流是指流入流出 ADC 输入端的直流电流，这个电流由器件内部 ESD 保护和其他寄生参数引入。在进行建模分析时可以把它看成一个直流电流源 加载到 ADC 的两个输入端口，类似于运放的输入偏置电流。通常规格书中给出的是典型值，但是实际中要比给出的值大很多，一般对于最大值的估计我们可以使用三倍的标准差来计算。  
![](https://img-blog.csdnimg.cn/895c5a147e6d432a95d0eb49f29d6be4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### **输入阻抗**

很多情况下 ADC 的输入阻抗是一个动态阻抗，动态阻抗是由输入漏电流和输入电容开关充放电的结果。  
![](https://img-blog.csdnimg.cn/f138c7d9e7554962843a18f123008771.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### **参考电压值**

定义为一个特定的参考电压值，通常这个电压作为此转换器最常用的参考电压。在参考输入电压范围内，使用任何其他参考电压值器件的性能与指定的电压值是相同的。  
![](https://img-blog.csdnimg.cn/97c1636b2b674de0af8816205a51eb5b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### **参考电流值**

![](https://img-blog.csdnimg.cn/111175696ce645c89c3436e99b70e8e2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### **微分（差分）非线性 DNL**

[ADC 相关参数之—INL 和 DNL  
](https://www.cnblogs.com/raymon-tec/p/4995698.html)

理想的模数转换关系如下：  
![](https://img-blog.csdnimg.cn/4a8a3ab8330a44f18ac8944c4d51435a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
理论上，我们用数字量的台阶去给模拟电压值进行编码的时候，台阶的宽度应该都是一样的，也就是说当 ADC 输入和输出是呈线性关系的时候，每次模拟输入按照最小分辨率 LSB 进行步进的时候，数字输出就增加 1，也就是 0000 变成 0001 的一个过程。

差分非线性 DNL：  
![](https://img-blog.csdnimg.cn/1f23f939e2a949d0800af3cae90f9516.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
DNL 是理想输入宽度和实际输入宽度之间的差额，输入宽度是产生同一数字输出代码的输入值的范围。由于 DNL 的存在，导致可能当数字输出由 1000 变成 1001 的时候，模拟值的变化却不是按照 LSB 进行增长的，可能会多一点也可能少一点，如上图所示。

**差分非线性造成的影响：**  
差分非线性可能造成失码或者非单调性，如下图所示。  
![](https://img-blog.csdnimg.cn/c94a06bdf0b5429eb42ec9b3e35ec2bc.png)  
差分非线性示例：  
![](https://img-blog.csdnimg.cn/7e8638a93d8040fcbb9f9b133173a254.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### **积分非线性 INL**

积分大多跟累计误差有关，根据实际的模拟出一条曲线。INL 是指 ADC 器件在所有的数值点上对应的模拟值和真实值之间误差最大的那一点的误差值，表示测量值的绝对误差。下图绿色虚线所示用的表示理想曲线方法是两点法，就是把头和尾用直线连起来。红色虚线是根据实际的情况模拟出的曲线，找到两个曲线纵坐标差距最大的点。  
![](https://img-blog.csdnimg.cn/79a2868f3c3143d58ebb70297d725d6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
![](https://img-blog.csdnimg.cn/0ee7583afb074442a9d029d443fd0c60.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
**理解 INL 和 DNL 的关系, 一句话, INL 是 DNL 的积分和，好的微分非线性并不能保证有一个好的积分非线性，但好的积分非线性可以说明微分非线性也不会太差。**  
![](https://img-blog.csdnimg.cn/d5827e3f3c234bb887937a1634f7a742.png)![](https://img-blog.csdnimg.cn/2faee8f227c8455abd01909eb7eac5de.png)

### **失调误差与增益误差**

AD 转换是一个非线性的过程，在计算中最常用的就是线性端点拟合。选择 ADC 的起始点和终止点  
作为拟合参数拟合曲线为 y=mx+b，斜率可以通过任意两点的坐标确定。  
失调误差（offset error）是当 x=0 时 Y 轴的截距 ；  
增益误差（gain error）是理论斜率和实际斜率的差值百分比 。  
失调误差和增益误差通常也被称为直流误差。  
![](https://img-blog.csdnimg.cn/e468ffa49b1a442f8b171c358cad1a92.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
**共模抑制比和电源抑制比**  
共模输入电压是指两个输入端的平均值，当输入电压发生变化时它将引入一个误差源，这里可以理解为一个失调误差源加载到了输入端，误差源的大小由共模抑制比来确定，即 CMRR。共模抑制比通常用分贝来表示，图中公式分别为共模误差和共模电压变化量。  
电源抑制比 PSRR 可以理解为电源产生的一个误差源加载到了 ADC 的输入端形成一个误差源 。  
![](https://img-blog.csdnimg.cn/6181487832224ff382d03ff6cbeffef2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
测量共模抑制比最简单的方法就是将两个输入端连接在一起  
![](https://img-blog.csdnimg.cn/3b41cf4b5cdd42ae9091ac4d0dfd31a9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
电源抑制比考虑的是电源电压变化引起的误差，这个误差可以理解为直流电源的变化量或者噪声信号。  
![](https://img-blog.csdnimg.cn/03ffd8dd15334b7c851db1a49dd2f0b8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
**信噪比 SNR**  
信号与噪声的比值，计算公式如下图所示。理想状态下只考虑 ADC 的量化噪声，可以推导出 SNR 理想值，公式如下，借此来评估测试系统 SNR 值的好坏。  
![](https://img-blog.csdnimg.cn/e1241c76672c47319be753d60018cac0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
**总谐波失真 THD**  
假设有以下模数转换曲线，可以看到在输入电压较低时非线性曲线与理想曲线非常的接近，但随着输入电压增大，曲线逐渐偏离，也就是说输入信号大时，实际的增益要比理想的大很多，这样就会把正弦波的上半周给拉长，类似这种现象就称为失真，在频谱中会引入谐波。![](https://img-blog.csdnimg.cn/d13419fd8d0544e9a98c9146782bfed1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
如果数字信号能够无失真的还原输入信号，则不会有谐波产生 。  
![](https://img-blog.csdnimg.cn/65124896e43f41ccac28c93d856707d5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)  
如下图公式，THD 是各个谐波电压的平方和除以电压有效值平方的均方根，可以用百分比来表示，也可以用 dB 来表示 。  
THD+N 与 THD 类似，但是在计算中包括了噪声的有效值。  
信纳比 SINAD，是信噪比和失真的简写，等于 THD+N 的倒数。 如果用分贝来表示的话，两者数字相同，符号不同。  
需要注意的是信纳比 SINAD 和 THD+N 通常要比信噪比 SNR 和 THD 差，SINAD 是在 SNR 基础上加入谐波，THD+N 是在 THD 的基础上加入噪声。  
![](https://img-blog.csdnimg.cn/abafd0e89a534e199045e200f1add7dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAa2VpbHpj,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

**有效位数（ENOB）**  
勿将有效位数 (ENOB) 与有效分辨率混为一谈，ENOB 是包括了量化噪声和失真项，有效分辨率用于衡量 ADC 在无量化噪声的直流输入条件下的噪声。将计算所得的 SINAD 值替换 SNR，并求解 N，公式如下图所示。看得出来就是 SNR 公式的变换。  
![](https://img-blog.csdnimg.cn/eab33b2f3bc74aa583e04de8299e3ba6.png)  
**无杂散动态范围（SFDR）**  
英文全称是 Spurious-Free Dynamic range，意为无杂散动态范围，反映了 FFT 分析频谱中信号幅值与最大谐波的距离关系。所以 SFDR 值越大则说明系统的噪声水平越低，ADC 的动态性能越好。单位 dBc 是相对于载波频率幅度，dBFS 是相对于 ADC 满量程范围。  
![](https://img-blog.csdnimg.cn/583cc14f79c747ad803040858ad5210e.png)  
对于高速 ADC，若要最大程度地提高 SFDR，存在两个基本限制：第一是前端放大器和采样保持电路产生的失真；第二是 ADC 编码器部分的实际传递函数的非线性所导致的失真。提高 SFDR 的关键是尽可能降低以上两种非线性。

![](https://img-blog.csdnimg.cn/85afaf7a97b64bb0b65f2e9f49574d23.png)