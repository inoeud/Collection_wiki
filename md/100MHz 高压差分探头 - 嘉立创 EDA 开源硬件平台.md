> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [oshwhub.com](https://oshwhub.com/adminismk/50mhz-high-voltage-differential-probe)

> 数码之家首发：https://www.mydigit.cn/thread-450270-1-1.html

数码之家首发：[https://www.mydigit.cn/thread-450270-1-1.html](https://www.mydigit.cn/thread-450270-1-1.html)

**简介：**  
你还在为囊中羞涩买不起高压差分探头而烦恼吗？

你还在为高压差分探头的可靠性而担忧吗？

这都不是问题，纯国产方案差分探头，它来了，它带着极致的性价比来了！

物料成本超低，仅 50 人民币不到！！！！！

**项目特点：**

*   本期将采用全国产器件的来制作一个 1200V 高耐压输入的差分探头
*   该探头实现锂电池供电与 type C 双电源使用，支持便携式使用。支持电源路径管理
*   采用的核心运放来自圣邦微的 SGM8061，一颗轨到轨的 CMOS 工艺的 500MHz 带宽的高速放大器
*   锂电池充电管理芯片使用常见的 TP4057，一颗线性充电芯片，设计充电电流为 220mA
*   隔离变换供电芯片是来自源特科技的 VPS8702，采用全桥架构外围精简，能提供 1W 的输出功率
*   高压输入采用 2mm 的香蕉头连接器，信号输出采用的是 SMA 内孔连接器，可快速拆卸线缆设计
*   体积小巧，性能强大，支持电池供电使用，支持快速拆装线缆，PCB 尺寸为 35mm*100mm
*   由于运放的限制，实际运放的输出摆幅是 ±2.3Vpp，可以更换为 AD8001ART，提高供电电压解决
*   方波振荡的原因可能是运放的问题，压摆率限制了性能，目前国内没有更好性能的运放了

**设计要求：**  

*   所有物料要求是国内的厂商
*   拥有 50X 与 500X 双增益档位切换
*   支持锂电池供电，支持边充边用，电源路径管理功能
*   共模抑制要求尽可能的高，以应对工频干扰
*   所有物料方便好买，物美价廉

**技术指标：**  

*   电源输入：4.8V~5.5V，使用 type C 接口供电
*   电池电压：满电电压 4.2V，过放电压由锂电池保护板限制
*   50X 增益带宽≥5MHz，500X 增益带宽≥50MHz
*   输入电压要求≥800V 且不击穿

**更新日志：**  

*   旧版勉强能满足使用，但其抖动略大
*   优化了共模抑制比，相对旧版有较大提升
*   优化了外壳文件，空间利用率提高，外观优化
*   V6 版本优化了负输入的信号回勾问题，更新外壳文件，外壳文件鸣谢 @xunmujun
*   由于发现买到假运放了，重新更新了测试数据，这颗运放的性能还是蛮强的
*   更换了测试设备，测试用的示波器由普源 MSO220A-S 更新为 DHO4204

**原理图简述：**  
**1. 充电与路径管理与电压指示**

![](https://image.lceda.cn/pullimage/d7R9tOhLFdf2oYHtLT870aGtM17m7XuwdoWKfMSk.png)

使用 D2 将 type C 输入电源分割成为两个电源域为路径管理行方便  
加入了 PD 握手识别电阻，支持 PD 供电头使用  
右手边的 LED 灯通过分压电阻进行控制，用于电源指示与电池电量指示  
其最低亮灯电压为 3.5V（很暗），该功能聊胜于无吧，能简易指示电池有无电量

 如果有高要求的话，可以使用专用的锂电池电量指示芯片，例如 HM1160 做电量显示

**2. 电池充电与简易路径管理**

![](https://image.lceda.cn/pullimage/sJ49dpb64cp4H1ItpczjL5PKLUHK7pjhAzUM3voY.png)

采用最便宜的 TP4057 芯片，厂商任选，成本极低，功能完善  
而电源路径管理没有找到合适的芯片，又或者是芯片太贵。只能手动切换电源路径控制  
在使用电池供电，且按键按下的时候，将拉低 Q2，Q3 的栅极电压使其导通，让其正常工作  
其二极管 D5 是为了防止电流倒灌，避免被 VPS8702 拉低影响该开关电路的工作  
在使用电源供电时，由 VBUS 电压经过 D5 强制拉高 Q2，Q3 栅极电压，禁止在外接供电时充电  
避免的 5V 供电直接对电池进行充电，实现这个操作的前提是 D2 这个二极管，这个二极管要求是压差低  
压差太大的二极管会导致实际到达 TP4057 的电压低，会造成充电电流低等问题

3. 隔离变换供电与稳压滤波  
市面上各种隔离电压模块很常见，但是体积很大，不符合我的要求

![](https://image.lceda.cn/pullimage/sh6HEQ0JWMGfgZTooJdZL5hguSFO2Fenmg2xf59y.png)

芯片厂商是苏洲的源特科技，主攻微功率的电源变换芯片，项目采用的是 VPS8702，SOT23-6 封装  
变压器采用的是 EPC10，该变压器功率容量最大 3W。对于这种场合完全不在话下  
由于应用对象是运放，需要双电源供电，将两个肖特基二极管并联为了降低压差  
增加 RC 串联电路以此降低反向恢复尖峰，降低电源纹波，降低 EMI 干扰  
U14 电容跨接在两个电源的地平面上，为变压器的高频干扰提供一个回流路径

![](https://image.lceda.cn/pullimage/V8E1v7hdDLgbzHfx9QgzI7TqSzesxzLzZj6WvQXl.png)

经过电容滤波的纹波高达 30mVpp，还是不满足要求，需要经过磁珠的二次滤波  
磁珠规格采用的是 2KΩ@100MHz，电流 200mA，其中最重要的是后面的两个稳压电阻。  
后端电路需要消耗一定的电流，使其磁珠产生压差才能让磁珠工作，经过磁珠之后，纹波降低到 10mVpp 了  
这个纹波完全可以接受，在经过 LDO 滤波还能降的更低。

![](https://image.lceda.cn/pullimage/7UDe27KvtxYt8gj290VfProDzMJIAjah0pUcpIzf.png)

正电源的 LDO 采用的是微盟的 ME6211C25M5G-N，是一颗固定 2.5V 输出的 LDO，其 PSRR=70dB@1kHz  
这个物料的纹波与 SGM2209ADJ 一样，平滑无毛刺

![](https://image.lceda.cn/pullimage/4c2CNKmM0z3Rs6hu7egq93AiuHKpZO6EpTKxwATL.png)

原本选用的是 SGM2019ADJ，但是它的纹波与数据手册完全不符，买了三次完全测得的纹波都一个鸟样  
之前怀疑是买到假货了，就去三个不同的店铺去买，还委托供应商拿了些样品回来测试，都一样  
不知道是不是批次问题，还是什么的，无解，只能换微盟的 LDO

![](https://image.lceda.cn/pullimage/j30X6gnhlkZyGqp10YMcwemx5NTIFhaFUJjZO1cw.png)

![](https://image.lceda.cn/pullimage/LWMgzHWETjT1e2HlNp46Q0QNd4Lwe6vmlGxz6qOj.png)

下图这个是 SGM2209ADJ 的 LDO，圣邦微一款负压 LDO，也是国内唯一能找到的高参数的 LDO  
其 PSRR=75dB@1KHz，国内算得上是很优秀的负压 LDO 了，我在国内其他厂商里面都没有找到负压的  
真的是一枝独秀啊，虽然价格很昂贵，但好歹买得到。换做 TI 的物料，价格翻了十几倍不止呢  
纹波表现可比 SGM2019ADJ 亮眼的太多了，平滑无毛刺

![](https://image.lceda.cn/pullimage/JyfT7fpLc3argyRFJNLGofsVHyoLBM1HkK1OSZof.png)

![](https://image.lceda.cn/pullimage/BL4xdWa1JYinz1enXlAKWgXUT802ApCzO60DbLiu.png)

电源处理到此结束了，供给给运放的电源电压是 ±2.5V，纹波是≤5mVpp 的  
受限于示波器底噪，高频变压器的磁干扰，不然应该是能达到手册上描述的 50uV 的纹波

**4. 电压衰减电路与放大器**  
一个 1206 电阻的耐压是 200V，使用多个进行串联以提高耐压，并联在电阻上的电容是进行相位补偿的  
衰减网络设计为 500:1，可以兼容市面上大多数高压差分探头，方便使用  
由于电阻的精度不能完全满足运放的共模抑制要求，所以需要一个可调电阻进行微调到合适的值  
但由于运放失调电压的存在，共模抑制调整好了，可能运放的输出轨道会偏，所设计了一个轨偏置电路  
信号处理电路的最后在加上二极管进行钳位，避免大电压冲击运放

![](https://image.lceda.cn/pullimage/ivBEP7IdDkCxsYwHvCTzS7Ikvf2XOpurHGat68SX.png)

运放采用的是 SGM8061，为 sot23-5 封装的单通道 500MHz 高速运放，采用 CMOS 工艺，是电压反馈型运放  
该运放的价格低廉，且性能强悍，是目前国内最强的运放了，其次是 GS8091，带宽是 350MHz  
使用三个高速运放组成仪表放大器，使其具有超高的共模抑制比，高精度输出

V6 版优化了反馈环路，稳定性更高，工作时抖动会降低，测量时精度会更高

![](https://image.lceda.cn/pullimage/7WGqz58mT53gdV0EJpuzJXE9mXsPQA5DpoTTt9k8.png)

但是不可避免的是，该运放的输入失调电压真的是太大了，会导致在 50X 增益时的轨道偏移  
我实际贴出来的板子，上面的 402 欧电阻用的是 0.1% 精度的，都挽救不了这种情况

![](https://image.lceda.cn/pullimage/RRBLpL4s6gb6y7u3GWujaEYk6STjeEPQWOSfpUOR.png)

U15，U17 运放组成同向放大器，在断开模拟开关时，增益为 1，接通模拟开关时，增益为 10  
由于是电压反馈型运放，所需的反馈电阻较大，同时对布线较为敏感，容易自激振荡  
所以加上了一个前馈电容，虽然稳住了环路，但是也降低了带宽，实属无奈之举

![](https://image.lceda.cn/pullimage/VlTr5GF6KnBuCSlhq2H9CnuEMuHtMmFZtHpcwUuP.png)

根据运放的数据表格，可以看到，在输出增益为 + 10 的时候，带宽是非常低的，只有不到 10MHz  
再加上前馈电容的影响，实际上还到不了 10MHz

![](https://image.lceda.cn/pullimage/97OyQ3hf76RBL9dRqoUgK9t8Dcxq8oqho6PciOpn.png)

最后的一级运放是作为输出级使用的，一个简单的差分转单端输出，输出端接了一个隔离电路  
这其实也是一个 RC 低通滤波器电路，避免运放负反馈环路与输出干扰，-3dB 带宽是 106MHz  
运放的负反馈上的那个前馈电容主要功能是调整方波信号过冲问题，副作用是带宽会降低

![](https://image.lceda.cn/pullimage/lWw7OBGM4vQfmrXw90GpE4l5xl0eql2TSzJ8dL0J.png)

模拟开关是来自上海的贝岭 BL1551B，最大导通频率是 300MHz，寄生电容约 5pF，略大了

V6 版本也对负信号输入进行了优化，使其回勾变小，工作频率可以更高

**5. 过压检测与告警电路**  
这个功能也是心血来潮加上去的，将 TL432 当作比较器使用，在低带宽的情况下响应是没问题的  
但工作频率增加之后，光耦是来不及响应的，会导致在高频信号下，功能属于残废状态  
不过对于一些低频的开关电源来说也是勉强够用的，这属于低成本的过压检测  
高级一点是使用高速比较器 + 计时器进行检测与告警，不过电路复杂成本高

V6 版本的原理图将 Q5 的 C，E 极给修正了，之前 B 站网友反馈说我接错了

![](https://image.lceda.cn/pullimage/BdGHFJafQ1OELjlXoddhY6W4uYzdIla5YvIQIigM.png)

---------------------------------------------------------------------------------------

**PCB 仿真图**

**![](https://image.lceda.cn/pullimage/aiCLCjovUwbY3Voj5ewDYeY27H0lu4gkgRxpN2xU.png)**

![](https://image.lceda.cn/pullimage/euJ9ZF2rqLAVde4FUMoz1n7ICDRVL9uUMD9Rs7Ml.png)

**成品实物图**

![](https://image.lceda.cn/pullimage/8gOHHcGrYahxPMPBjZVV2BMBFhXOwkVaIXWuNDdT.png)

![](https://image.lceda.cn/pullimage/posAKjDBD55LWWjteosuPnTWsbPsqmp0goT94ofw.png)

![](https://image.lceda.cn/pullimage/he6MG0JEWh2LFj7FHn2rB9RM6ULds0vbxKyEGPRT.png)

![](https://image.lceda.cn/pullimage/kj58vWvSXS3upPe7ewoOFgh6RmRubZpRztCDMEO3.jpeg)

**细节部分**

![](https://image.lceda.cn/pullimage/sHIpFOJ5n7ZcnW69d8ZKIL0mlkMOJx6uKCUGEgy5.png)

![](https://image.lceda.cn/pullimage/iQDuqn7BVdkl7L0l8hyiezDHRAtqkVge8vekhjRL.png)

![](https://image.lceda.cn/pullimage/RIXyzyih1e3xgGdpx8vzUOABPi0fOYcUarqMakRP.png)

**外壳实物：**

电池采用 502030 规格的，能满足使用一个半小时的便携式使用，如果觉得电池容量小，可以选用 602040 规格的

第二版本的外壳，优化了空间利用率，优化了外观，更美观，更符合产品的定位

![](https://image.lceda.cn/pullimage/TrLyAXC6uJfHJT2tuOCLqPEuEHrAa8I6KTFJz241.png)

![](https://image.lceda.cn/pullimage/bPEI1kHzEWgJyZjkqesus0ey2XISXIO0jI6AG3sj.jpeg)

![](https://image.lceda.cn/pullimage/cs9vfWdi3Im4k9XKOdJraBN4XkntRs7Cn2vNEFL4.png)

![](https://image.lceda.cn/pullimage/HBhGccrESHLT687mZ1CHK86VOYIvUfzZa4Br15A7.jpeg)

![](https://image.lceda.cn/pullimage/02zP5nAO2fbt1LDZGZIffpCNpsihLzx6kwpwtsVI.jpeg)

按键是选的成品按键，外径 5.9mm，高度 10mm，后面改版外壳的说不定会变

![](https://image.lceda.cn/pullimage/VD8ZbfLiYlxEt756rYE4ZH1No2HcsHskXroSjU0R.png)

![](https://image.lceda.cn/pullimage/YvgeOAU5KWrTMGb3DlKR9YcQ3yfyAKaDYYd8Ow7J.png)

螺丝选用的是 M2 规格的沉头螺丝，长度为 8mm

![](https://image.lceda.cn/pullimage/3aHLVRgBzV8zaocEf1v4JctjUVMsyfTqi0WxECSN.png)

---------------------------------------------------------------------------------------

**调试经验分享：**  
主要调试是在运放部分，其他的电路已经由我调整验证完了

![](https://image.lceda.cn/pullimage/DbWXiEJvPuB8AtqkuOVOFJ4qlaW1u05EZQrRQtN6.png)

![](https://image.lceda.cn/pullimage/EOXxQWAXWuJL5NXZoyuutPgssyGN4miQ1hXvTeQi.png)

1. 第一步先调试共模抑制旋钮，需要先把正负输入的引脚短接了，  
然后将其共同接到其他设备的外壳上此时会出现一个 50HZ 的工频信号（其他设备外壳不能接地，否则是没有的）  
有条件的可以用信号源产生，调整示波器到 1X 衰减，直流耦合状态，调整到 50ms 时基，2mV 的垂直档位  
逐渐的旋转微调旋钮，直到工频信号消失  
这个时候信号轨道还是会偏移的，就需要调整另外一个旋钮了，使其调整到接近 0V 上  
这个是 500X 增益时最终的效果，由于失调电压的存在，还是存在一些微小的波动的

下图是旧版的共模抑制实际表现效果

![](https://image.lceda.cn/pullimage/86pjYy3E298oNiaRFsy9B1q8tjDUpGiwhKEFgjto.png)

下图是 V6 版本的表现，发现其抖动状态消失，状态良好，这个是正品运放的短路输出纹波

![](https://image.lceda.cn/pullimage/3OTteLiUIiNI5P3tqnZohBS8PrBbZfH2cCMjoZso.png)

这个是 50X 增益时的效果图，可以看得见，电压轨道偏移的厉害，而且共模抑制较差，波动很大  
此时再调整轨道偏移也没什么意义了，因为失调电压的存在导致两者不能完全兼容

![](https://image.lceda.cn/pullimage/UfjUPoqV7HPkXMAp2R6ljUGfeE55bvjWLaBVLL48.png)

虽然有点缺陷，但是输出噪声水平已经接近了麦科信的 100MHz 高压差分探头的水准了

旧版的抖动较大，下张图是 V5 版本的，抖动已经消失了，但是轨道偏移还是存在的

![](https://image.lceda.cn/pullimage/gSxlj8r4aCPBwkzjgJsZNVitG1DvabH6g41zKpdr.png)

2. 第二步是波形补偿调整，用信号发生器产生一个 100KHz 的方波信号，幅值为 10Vpp 的方波信号  
先将信号正接，逐渐调整到最佳补偿，然后去调试负信号的补偿  
直到两个信号补偿如下图所示，此时信号补偿调试完成

![](https://image.lceda.cn/pullimage/eS13B4eotr7Qx9x2xthXj7TZ78AxyEArQTrAszJO.png)

3. 接下来调试 50X 增益时的放大倍数  
用信号发生器产生一个 100KHz 的方波信号，幅值为 10Vpp 的方波信号  
将示波器衰减调整为 50X，将差分探头也调整为 50X，逐渐旋转微调电位器，直到 Vpp 与信号发生器相等  
信号发生器的幅值最低不要低于 5Vpp，要不然经过 500X 衰减网络之后可能只剩下噪声了  
建议选择交流耦合，这样子可以避免直流偏置的影响，此时信号衰减补偿都调试完成，可以正常使用了

----------------------------------------------------------------------------------------

**50X 性能指标测试：**  
这个就不测正弦波了，没什么意义，反正带宽也不大，直接测试方波了  
单独测试吧，避免被麦科信虐的太惨，实际 - 3dB 带宽接近 5.5MHz，这个主要是受限于运放带宽，压摆率，建立时间  
使用信号源输出方波信号，幅值为 5Vpp，逐渐增加到其失真无法保持，直到变成三角波

![](https://image.lceda.cn/pullimage/VsMFE15xj4ujUfkeMBFwFHPgT0SqsT4a39qtCs0e.png)

![](https://image.lceda.cn/pullimage/xlTeDlLTH2mAYbXP4EwetEuo7AbJa3nlKwnAEJOa.png)

![](https://image.lceda.cn/pullimage/WxoHZUewksc3NeEAUxtk5WtmIFdQWpN83gjGwHB3.png)

![](https://image.lceda.cn/pullimage/2Td0Kl0w0oyXDzr97E1l7VMlsPGSUxpA51vYB1NW.png)

![](https://image.lceda.cn/pullimage/uSaXFuaGoWfgksmiIK7P9qG1XedOmaQefoEbxvHF.png)

![](https://image.lceda.cn/pullimage/of1GFtRoVeLy78GGKjqltBVhTGY83YV8oJ4mqXQh.png)

![](https://image.lceda.cn/pullimage/SntSkmNjRF0ISFTEF3HXmzhCIUojCWuEpS11qBw5.png)

![](https://image.lceda.cn/pullimage/aRRzemBENJUKpVngj2oTJmwSqOGia1m4aGwIcXfJ.png)

![](https://image.lceda.cn/pullimage/gEGvmaMxmiAwnM5dUMXZpvY7GI6zHNs2bX9vRqTE.png)

![](https://image.lceda.cn/pullimage/08Po8JE68BK89b1QZeVBaGx6QZXsIKPZrm6KikOE.png)

![](https://image.lceda.cn/pullimage/5KCgYUTvuU0pZNCX9Df5H7HzQm09MXmc56tnocl5.png)

![](https://image.lceda.cn/pullimage/USyQBLCNvXkFtDJkE8fv8tb4k7z1qjmvXVSHidND.png)

![](https://image.lceda.cn/pullimage/hlxsNo4CaC7XcbtVwWKNnqlb8s5r0AVzNkb4umNR.png)

**500X 正弦波性能指标测试：**  
黄色是麦科信的 100MHz 高压差分探头，蓝色是我制作的高压差分探头，统一使用 500X 衰减  
使用信号源输出 10MHz~100MHz，幅值为 5Vpp 的正弦波，可以看到我制作的差分探头 - 3dB 带宽是 100MHz  
在 100MHz 时的相位超前是 3.55nS，这个是可能是由运放的 C35 电容引起的，也可能是运放本身的原因

![](https://image.lceda.cn/pullimage/5jZgK9hsmyKGEySU2EsvM78Xfp0IzFZbUTlJ7l0m.png)

![](https://image.lceda.cn/pullimage/tplR85MXrMaxs2EH4lfkiEiAgqoCNNP1wffDdWvE.png)

![](https://image.lceda.cn/pullimage/XMRbXesSvFotADwpM1uQ5IJTCYIZd12KulWKfJHB.png)

![](https://image.lceda.cn/pullimage/Ew2GcikVeMXYcX3bAHNijqmfuaveBrksswAOcoV9.png)

![](https://image.lceda.cn/pullimage/mWyivaxiy08klK8j403i4HNd3yXBMgI9qILVPyDe.png)

![](https://image.lceda.cn/pullimage/9FGxzMS9Qvwt2n0sH1I0brjxMA9qzfFzsdb60Ylr.png)

![](https://image.lceda.cn/pullimage/b38zJoX1TIeCJkOTcX00E4ampMNxm5hSbj2IJzQe.png)

![](https://image.lceda.cn/pullimage/g2jqOZSPsBtw2lTbzfQMu2NZWodJjMSfSbi5k7zu.png)

![](https://image.lceda.cn/pullimage/WrwWlzSEDEQlNJTswIvHegG0OrPP4ZB3gJLwvQqn.png)

![](https://image.lceda.cn/pullimage/tO4oXaI8S03kwLOSpuosG7PsJDYpjWoqA8fe6tcF.png)

![](https://image.lceda.cn/pullimage/jLy5GAuYsI6jG3KhGT4leRkJcM34SjrLhAnROEWW.png)

![](https://image.lceda.cn/pullimage/XeNzPdvcZRZTlwUl6gvI05m8NPEr8Dejftd4Eo2f.png)

**上升沿性能测试：**  
使用信号源输出 1MHz~20MHz，幅值为 5Vpp 的方波，使用 500X 衰减  
黄色是麦科信的 100MHz 高压差分探头，蓝色是我制作的高压差分探头，统一使用 500X 衰减  
在 10MHz 的方波下可以看到相位超前是 4.65nS，麦科信 100MHz 的上升时间为 5.65nS，我的为 5.45nS

正品的 SGM8061 的性能指标是真的很不错啊，比之前那个冒牌货强很多了

![](https://image.lceda.cn/pullimage/INkMEcOSZQSv9pzTspuFlsUlesGH778Anw1afxnG.png)

这个是实际测试出来的方波，因为挖空了运放的负反馈地平面，会导致阻抗变得不连续，反射情况比旧版严重一点

![](https://image.lceda.cn/pullimage/poSXBT1mRYshjK5gDoiiTBz5LerL4DK8f1fpZxaZ.png)

![](https://image.lceda.cn/pullimage/dvMMDV3dyKPFZAgIbxNJVgjR7zYL3y8MIZBG80Qq.png)

![](https://image.lceda.cn/pullimage/hGH2sdkHI15QON4UuBNG5pqp6FQP1BATrSj51jcQ.png)

![](https://image.lceda.cn/pullimage/NTb7GLDAvf6vQ72IxJf0E4Z4Aw3IbIt82dhUTxsE.png)

![](https://image.lceda.cn/pullimage/sa4TCmA0BLQz1RhyujitiOdLwTtW6Ri0cRZKPfhT.png)

![](https://image.lceda.cn/pullimage/DNFfElfip4alWIoOY71vFgkhm6iOHedkGTHe0lcC.png)

![](https://image.lceda.cn/pullimage/xl1ry3s76QmPKYeDxoZBf89NAFq9vzzr4vB1EPTk.png)

![](https://image.lceda.cn/pullimage/2KDPYtpwjPMYUIFi2E34RvMwCZQKALEkxVzLdFEO.png)

![](https://image.lceda.cn/pullimage/aV5liDzRSRb7vKJ5IOHm4QmSiFCSrS0onFylvlb8.png)

![](https://image.lceda.cn/pullimage/GqbNs1Rk3jLBq9jZ6ZVw1alSHA8bKiEASXhAtfFs.png)

![](https://image.lceda.cn/pullimage/tj8IJqYfzrXd7K71HVqSojNRyvXsjEABBiHOhjTE.png)

![](https://image.lceda.cn/pullimage/J5SPue7ViEez28GiyFO2cL0kssY9YZOrHo20pdTA.png)

![](https://image.lceda.cn/pullimage/Vogwot27vAqXqURyfnHPNfQX74y9o9TRe4SYF6KY.png)

![](https://image.lceda.cn/pullimage/Bj8FmlIxshCmsKloFwNL42LmnP1XMpQ3M9aamGJp.png)

![](https://image.lceda.cn/pullimage/uY5rQFeql4zJVtDQxxuOdZSYVjZBEu1PBHNFyHMU.png)

![](https://image.lceda.cn/pullimage/SP8ajow1kLFl8D4TuCMa6sBitPZiWldwAvLvbOYr.png)

![](https://image.lceda.cn/pullimage/7a3iCPtQDMqbOOKMRGWFaSykxE3BSMyYiscotFeB.png)

![](https://image.lceda.cn/pullimage/VfBVldOVKvYo4wYaakz2VGONZ0Gwi1b2IP9CvBw0.png)

![](https://image.lceda.cn/pullimage/1m3VUiQ2KElt2DOwUEjBSRdcEVySs44QHl4ngylZ.png)

![](https://image.lceda.cn/pullimage/fJ8DcZm6F4LwyPaiNvqjI2Xc7DjhQ6GX6nkfN9z3.png)

**500X 正向输入方波性能指标测试：**  
使用信号源输出 1MHz~20MHz，幅值为 5Vpp 的方波，使用 500X 衰减  
黄色是麦科信的 100MHz 高压差分探头，蓝色是我制作的高压差分探头，统一使用 500X 衰减

更换了正品运放之后，解决了运放相位滞后的问题，现在是相位超前了，在 10MHz 处超前 4.6nS

![](https://image.lceda.cn/pullimage/DiyRKaIZch1crHmXqjl0o1qqlWBQyGyBkDmYfHn6.png)

![](https://image.lceda.cn/pullimage/Rkjz5exd92fIldUsK4Zqmni0GuhEGXnFqxsdNuRV.png)

![](https://image.lceda.cn/pullimage/MBEpxmEbLWVULPnDxfvUPhVNRodu8YBKZ8hkXGJd.png)

![](https://image.lceda.cn/pullimage/178gFFSnvKVJyGiWr6XovVnToJ456Ccd67dEjiO8.png)

![](https://image.lceda.cn/pullimage/eKOoS9rJqWLteLWCxe61PqvI18wG1UU3PZAflqDE.png)

![](https://image.lceda.cn/pullimage/sLr1z6tY35lomky7Bx0nHPIdAa0TFgxqrNUixEAM.png)

![](https://image.lceda.cn/pullimage/SeecsuKd55sxs8tVC1ZA1p3aWLTe53WwQqv0ldm1.png)

![](https://image.lceda.cn/pullimage/pg1xuc6ojQdcVNBWVTjZqSS5JMldBr69lC3mDhqp.png)

![](https://image.lceda.cn/pullimage/IdL93F31x7O8Tx1MZr1Udce11eKO03r4IIdfGzvE.png)

![](https://image.lceda.cn/pullimage/pNJ4m1FbXZJ8V1k0Vs52s3quGCCoJhqLa35YDwq5.png)

![](https://image.lceda.cn/pullimage/2fVNvBBlvnXYnaU25rYhwXZBpXZm8nHcB36yWdmS.png)

![](https://image.lceda.cn/pullimage/ykOOT0C7FhUQV9ZIIKEmpjeQJDEUXOTejqQ9zLvR.png)

![](https://image.lceda.cn/pullimage/XL1HYFAJtpEgX0efOl7Z2fRNhUlbhoxjiRwtrrG6.png)

![](https://image.lceda.cn/pullimage/f7rhZ90kU2Mdov6rjdYYJLvZEzicUZT4Sa0956Zr.png)

![](https://image.lceda.cn/pullimage/mQTObh3waJk9olFPNQiVC2xSEMSUtRZTx6ANqrTb.png)

![](https://image.lceda.cn/pullimage/NNnSUUs36FyAicrt4c5CWi8JN6KrHiNBZGYd7l2b.png)

![](https://image.lceda.cn/pullimage/YRXyGTZ95mSCD1ufnZMPizUcRrrElntSQ1nh4bfV.png)

![](https://image.lceda.cn/pullimage/A6nGuPO7kyLmlYMbDyRz1o4vCE1hOvhwnYi1odAO.png)

![](https://image.lceda.cn/pullimage/CMQiOr1vPtaTnUn80RwMhmkM5RrMm00Hza2u5ZXn.png)

![](https://image.lceda.cn/pullimage/5Orof6Ds91KJQWcRYndFXJMV0ydMwVObhM7YwarZ.png)

**500X 反向向输入方波性能指标测试：**  
使用信号源输出 1MHz~20MHz，幅值为 5Vpp 的方波，使用 500X 衰减  
黄色是麦科信的 100MHz 高压差分探头，蓝色是我制作的高压差分探头，统一使用 500X 衰减

反向输入是对运放的重大考验，性能相对来说会差一点，反向输入 10MHz 处的相位超前 3.8nS

![](https://image.lceda.cn/pullimage/qJrnvooPxRfumAgObQ1MwIyih3nsSX5LhocLbfAy.png)

![](https://image.lceda.cn/pullimage/4r0ryoclluBTiyPhABMdx2avHn6t2vqXirdJvJQL.png)

![](https://image.lceda.cn/pullimage/hOgtaY3S7jzCLraop7xkDYw5EiSieoa5NLeQgEYe.png)

![](https://image.lceda.cn/pullimage/yrVQUn14EfYghGUpCQmXDbPyf6OXvsYom5rQRHX3.png)

![](https://image.lceda.cn/pullimage/FNDdgpB2VR4mET4txEdych9LdVj5FKTOElN8Pfkd.png)

![](https://image.lceda.cn/pullimage/zmxF9lAcBV2ZEbBesfuS8pZWiHBtRwJpGg4tKUvr.png)

![](https://image.lceda.cn/pullimage/P4n2KcHGVqCrFVtxKFCVNX1yLaoN47O0j1FSu7CL.png)

![](https://image.lceda.cn/pullimage/hrmWPHXinpinQkRznH75ZT810J2JqiyIjJd4R1W9.png)

![](https://image.lceda.cn/pullimage/c7fYhShiQtOXp3KldrEWZQQGzhmcDj1aRP4D14sx.png)

![](https://image.lceda.cn/pullimage/1KJ6JkXNeIm4Ck1zheFsJ9xoteXOUUrg3NaxXHEY.png)

![](https://image.lceda.cn/pullimage/my1SFh7kboTRdzqfan5n3Z5rxtnx1U8h1bTwwCjU.png)

![](https://image.lceda.cn/pullimage/PFKrBzkctwDvKPiHbUdRIn5Hm90mBer65bSsb8Ti.png)

![](https://image.lceda.cn/pullimage/TcOoLRsg8DvMgKrLaMK76GPROoQpaxY2f3pIGJE4.png)

![](https://image.lceda.cn/pullimage/gKSqB5uLYW9d4bGkMT9gITavIYFIB343OFQGwRbl.png)

![](https://image.lceda.cn/pullimage/o93qQ2dvXqeTN0CwaN6Lmhz6oY22EfnpWKvyg5M9.png)

![](https://image.lceda.cn/pullimage/daMooEmngGDCST7C7bbHcw0QWUhynYbEuvdH3jEi.png)

![](https://image.lceda.cn/pullimage/lT4CvA8hLCjyTPsPcRhibmAjLYdIbsYb4EaJPnQh.png)

![](https://image.lceda.cn/pullimage/llk7BrHCqOzpp630UBWVb8SpLHtdSzzeFAETY4Eo.png)

![](https://image.lceda.cn/pullimage/zsIJO1b61A4KRRO7KWwcSpEAp6HiDeIqY1DFuXFp.png)

![](https://image.lceda.cn/pullimage/AbO6QQRUVs4zZARHTM0ftLmRfVgxDNYwKWOOQfJZ.png)

反向信号输入的表现还算是可以的，不过在 16MHz 处发生了严重的信号回勾，导致信号失真了

**扫频测试：**  
使用信号源输出 100KHz~100MHz，扫描时间 20 秒，幅值为 5Vpp 的方波，使用 500X 衰减  
黄色是麦科信的 100MHz 高压差分探头，蓝色是我制作的高压差分探头，统一使用 500X 衰减  
正接信号输入，扫频曲线如下所示：

![](https://image.lceda.cn/pullimage/Y7UX4ZT8s7bjPaTBqi9o3YCFdalmjAp5f26cp8zW.png)

反接信号输入，扫频信号如下所示  
在约 25MHz 处发生了衰减，导致信号输出幅值降低

V6 版本的反向输入曲线略强于 V5 上一个版本，这是事实，不能同时兼得

发生这种情况的原因是运放输出端的 C35 前馈电容导致的，这是一个取舍问题，既要保证方波的过冲降低，又要影响小

![](https://image.lceda.cn/pullimage/GQJKlLPeZkd0DY1EsjtQDkFFmhKODvcC4vDEGg7e.png)

  
**通过性测试：**  
使用信号发生器产生一个 1MHz，5Vpp 的方波信号，打开示波器的通过测试，然后创建规则  
将差分探头调整为 50X 衰减下开始测试  
测试结果还是挺乐观的，几率低只能说明抖动不大，但实际上加载信号上是很明显的  
新版本的减缓了抖动问题，曲线较为平稳，这是一个良好的表现

![](https://image.lceda.cn/pullimage/C8957z338XTNgLRnSL7oZg3TXV5jZvfmj68TUriW.png)

![](https://image.lceda.cn/pullimage/UwJKwlk1Rw7WF7HS8MTVhIdyipAotJ7NFdMqhHj6.png)

---------------------------------------------------------------------------------------

总结：又不是不能用，要什么自行车

之前测试过了，使用磁环做变压器的方案不可行啊，，建议大家还是买磁芯自己绕制吧

原理图文件与 GERBER 文件带 V5 字样的是新版的，旧版的调试过程都在数码之家了

**后续 1：优化运放反馈地平面与环路**

V6 版本的反向输入也存在回勾问题，但这版本对信号进行的一定的优化，使其影响减小，回勾位置更上一点了

调整了 C35 的电容容值，从 22pF 更换成 2.2pF，输入正向输入的峰峰值变低了一点点，但有效的缓解了反向信号的回勾问题

![](https://image.lceda.cn/pullimage/glckgubllwaVBPdlZwblvBihrVHIaovQE62FbCdp.png)

![](https://image.lceda.cn/pullimage/ogCuHj3UQe7CMUbzC1vEAIOZU0bLhrou6FZLmbyS.png)

![](https://image.lceda.cn/pullimage/AnmQJoBcjzKdxaFDZWljbcUkuSPdLpeA8GhtULoy.png)

东科 65W 电源实测，黄线是麦科信 100MHz，蓝线是我制作的

![](https://image.lceda.cn/pullimage/pDhVqgtSQKCkNXopcdQsuIEz1PSG39vDjG6DlEaT.png)

![](https://image.lceda.cn/pullimage/G182om67tzpxLFX0ihncOn2dfdz6KKPNDTw4kVG7.png)

![](https://image.lceda.cn/pullimage/7RX8KrlmrFaU221UHmizNGTwVxJUgs7iKNaV9jRD.png)

![](https://image.lceda.cn/pullimage/AzITY3qditxc0lPhefFniqCR2ytdU9Pg2PizHs5F.png)

![](https://image.lceda.cn/pullimage/0rJbLKHlVcl1icxSSTcWlEkxFJV5zwC0Z4LzAVRc.png)

示波器打开了深存储，可以看到虽然有点小振荡，不过并不影响实际测量结果

反向输入实际测试

![](https://image.lceda.cn/pullimage/QqBR4IuqUJEglSoEYwFQFJ98gWAYju4HC2ACDZqV.png)

![](https://image.lceda.cn/pullimage/WmkonKSlmW352TRuTEfN085lbnsE1d8FKLlawuic.png)

![](https://image.lceda.cn/pullimage/gFXIMOFnXLpi82MlwM27CsA9rc6Dm4psSudVfiHS.png)

![](https://image.lceda.cn/pullimage/DXp9TaVSL5JGM7LXARSEuLBPBDgTeDN1YH6qIV20.png)

效果基本上一模一样！！！！

**后续 2：GaN 管驱动实际测试（更新测试设备）**

这个是测试之前我做的茂睿芯的氮化镓电源的栅极驱动情况

黄色的麦科信的 MDP1500 高压差分探头，蓝色的是我自制的高压差分探头

现在是正向输入情况反馈，可以看到，我制作的底噪相对于麦科信的底噪更低。

![](https://image.lceda.cn/oshwhub/9261b9e8041448e58e479563b433987d.png)

![](https://image.lceda.cn/oshwhub/ffd1144979c54aa891f3bb5a643569f6.png)

![](https://image.lceda.cn/oshwhub/9f785aaa7dec474e8707f07ce8d6dd1b.png)

![](https://image.lceda.cn/oshwhub/f1dba80fd5ee4c1a928f8e87ea5f226c.png)

![](https://image.lceda.cn/oshwhub/1af969e902454c7ba8b9494228636432.png)

反向输入对比

![](https://image.lceda.cn/oshwhub/bd9e100d9b124fb185072c6299b6836b.png)

![](https://image.lceda.cn/oshwhub/6d5700e77b08498bab7b22586079b972.png)

![](https://image.lceda.cn/oshwhub/542bedb8ec514883a444d15d07a6b867.png)

![](https://image.lceda.cn/oshwhub/bda833060f4f4320943c58a7b4dce348.png)

![](https://image.lceda.cn/oshwhub/f103d3f6d9714595a6a1365303f2aeaf.png)

![](https://image.lceda.cn/oshwhub/c0b9793b35d24503b6fac11d0ab4a889.png)

![](https://image.lceda.cn/oshwhub/5d574ca03bc146e8abbbc8e911b49ed2.png)

![](https://image.lceda.cn/oshwhub/e202539c1b424c038b745b9e06b1ff4d.png)

反向输入存在振荡，这个不好解决，目前暂为发现原因，所以尽量不要使用反向输入

现在测试三款高压差分探头

![](https://image.lceda.cn/oshwhub/b610751982024e6b82ecceedf0da05fa.jpg)

![](https://image.lceda.cn/oshwhub/89a1d8efc0014b729b9da484074bd95c.png)

蓝色的是麦科信 MDP1500，黄线的是东儿科技的 DR103R102，紫色的是我自制的，在三种探头里面，我的探头底噪最低。

![](https://image.lceda.cn/oshwhub/80cc7cd5462e49adb782e46d100f5e16.png)

![](https://image.lceda.cn/oshwhub/047e47e0980e45d0b83c22741f5f23ab.png)

![](https://image.lceda.cn/oshwhub/89816b3401994587958366d7c2c1fd40.png)

可以看到，我的底噪是最低的，使用的是一个手机充电头供电，另外两个探头都是从示波器上取电的

但是我这个探头对于共模干扰的抑制还是比较差的，解决办法是增加共模电感与差模电感进行滤波

主要原因是充电头太劣质了，共模噪声很大，导致底噪纹波也偏大大。电池供电好很多

直接插在示波器上进行取电，就没有共模的毛刺噪声了。

蓝色的是麦科信，黄色的是东儿科技，紫色是我制作的，仅供参考

![](https://image.lceda.cn/oshwhub/1d48db08453a4bac8135bb2dabd9d1da.png)

![](https://image.lceda.cn/oshwhub/7e28bd0777f84127b425320283a729ce.png)

最后来一张扫频曲线图，100Hz 到 120MHz 扫频

![](https://image.lceda.cn/oshwhub/4322f04003704c948369a0438bb78d50.png)