> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/498514949)

外设接口（SPI）是微控制器和外围 IC（如传感器、ADC、DAC、 移位寄存器、SRAM 等）之间使用最广泛的接口之一。

SPI 是一种同步、全双工、主从式接口。来自主机或从机的数据在时钟上升沿或下降沿同步。主机和从机可以同时传输数据。SPI 接口可以是 3 线式或 4 线式。本文重点介绍常用的 4 线 SPI 接口。

**接 口**

**4 线 SPI 器件有四个信号：**

*   时钟 (SPICLK,SCLK)
*   片选 (CS) 主机输出
*   从机输入 (MOSI) 主机输入
*   从机输出 (MISO)

产生时钟信号的器件称为主机。主机和从机之间传输的数据与主机产生的时钟同步。同 I2C 接口相比，SPI 器件支持更高的时钟频率。用户应查阅产品数据手册以了解 SPI 接口的时钟频率规格。

SPI 接口只能有一个主机，但可以有一个或多个从机。图 1 显示了主机和从机之间的 SPI 连接。

![](https://pic3.zhimg.com/v2-8cf21654ab7151bf5f3352a1d5978d72_r.jpg)

来自主机的片选信号用于选择从机。这通常是一个低电平有效信号，拉高时从机与 SPI 总线断开连接。当使用多个从机时，主机需要为每个从机提供单独的片选信号。本文中的片选信号始终是低电平有效信号。

MOSI 和 MISO 是数据线。MOSI 将数据从主机发送到从机，MISO 将数据从从机发送到主机。

**数据传输**

要开始 SPI 通信，主机必须发送时钟信号，并通过使能 CS 信号选择从机。片选通常是低电平有效信号。因此，主机必须在该信号上发送逻辑 0 以选择从机。

SPI 是全双工接口，主机和从机可以分别通过 MOSI 和 MISO 线路同时发送数据。在 SPI 通信期间，数据的发送 (串行移出到 MOSI/SDO 总线上) 和接收 (采样或读入总线(MISO/SDI) 上的数据)同时进行。串行时钟沿同步数据的移位和采样。

SPI 接口允许用户灵活选择时钟的上升沿或下降沿来采样和 / 或移位数据。欲确定使用 SPI 接口传输的数据位数，请参阅器件数据手册。

**时钟极性和时钟相位**

在 SPI 中，主机可以选择时钟极性和时钟相位。在空闲状态期间，CPOL 位设置时钟信号的极性。空闲状态是指传输开始时 CS 为高电平且在向低电平转变的期间，以及传输结束时 CS 为低电平且在向高电平转变的期间。CPHA 位选择时钟相位。

根据 CPHA 位的状态，使用时钟上升沿或下降沿来采样和 / 或移位数据。主机必须根据从机的要求选择时钟极性和时钟相位。根据 CPOL 和 CPHA 位的选择，有四种 SPI 模式可用。表 1 显示了这 4 种 SPI 模式。

![](https://pic2.zhimg.com/v2-2ba34b23ffc571115ddd3d57dd043155_r.jpg)

  
图 2 至图 5 显示了四种 SPI 模式下的通信示例。在这些示例中，数据显示在 MOSI 和 MISO 线上。传输的开始和结束用绿色虚线表示，采样边沿用橙色虚线表示，移位边沿用蓝色虚线表示。请注意，这些图形仅供参考。要成功进行 SPI 通信，用户须参阅产品数据手册并确保满足器件的时序规格。

![](https://pic3.zhimg.com/v2-ddbd0c1068a6b3076f1c6638205de792_r.jpg)

图 3 给出了 SPI 模式 1 的时序图。在此模式下，时钟极性为 0，表示时钟信号的空闲状态为低电平。此模式下的时钟相位为 1，表示数据在下降沿采样 (由橙色虚线显示)，并且数据在时钟信号的上升沿移出 (由蓝色虚线显示)。

![](https://pic3.zhimg.com/v2-c13eea7e5933df5859c3c17fad12201a_r.jpg)![](https://pic3.zhimg.com/v2-d33afe92072a004ffdf57bf1b6bf8d4e_r.jpg)

图 4 给出了 SPI 模式 2 的时序图。在此模式下，时钟极性为 1，表示时钟信号的空闲状态为高电平。此模式下的时钟相位为 1，表示数据在下降沿采样 (由橙色虚线显示)，并且数据在时钟信号的上升沿移出 (由蓝色虚线显示)。

![](https://pic4.zhimg.com/v2-5a3a177401f028996330be66d470009b_r.jpg)

图 5 给出了 SPI 模式 3 的时序图。在此模式下，时钟极性为 1，表示时钟信号的空闲状态为高电平。此模式下的时钟相位为 0，表示数据在上升沿采样 (由橙色虚线显示)，并且数据在时钟信号的下降沿移出 (由蓝色虚线显示)。

**多从机配置**  
多个从机可与单个 SPI 主机一起使用。从机可以采用常规模式连接，或采用菊花链模式连接。

**常规 SPI 模式**

在常规模式下，主机需要为每个从机提供单独的片选信号。一旦主机使能 (拉低) 片选信号，MOSI/MISO 线上的时钟和数据便可用于所选的从机。如果使能多个片选信号，则 MISO 线上的数据会被破坏，因为主机无法识别哪个从机正在传输数据。

从图 6 可以看出，随着从机数量的增加，来自主机的片选线的数量也增加。这会快速增加主机需要提供的输入和输出数量，并限制可以使用的从机数量。可以使用其他技术来增加常规模式下的从机数量，例如使用多路复用器产生片选信号。  

![](https://pic1.zhimg.com/v2-31871a8705ce3f001635317fa3b6a7cc_r.jpg)

**菊花链模式**

在菊花链模式下，所有从机的片选信号连接在一起，数据从一个从机传播到下一个从机。在此配置中，所有从机同时接收同一 SPI 时钟。来自主机的数据直接送到第一个从机，该从机将数据提供给下一个从机，依此类推。

使用该方法时，由于数据是从一个从机传播到下一个从机，所以传输数据所需的时钟周期数与菊花链中的从机位置成比例。例如在图 7 所示的 8 位系统中，为使第 3 个从机能够获得数据，需要 24 个时钟脉冲，而常规 SPI 模式下只需 8 个时钟脉冲。

![](https://pic3.zhimg.com/v2-47a3bf48fbc22aad1c3b320d0c7e5122_b.jpg)

图 8 显示了时钟周期和通过菊花链的数据传播。并非所有 SPI 器件都支持菊花链模式。请参阅产品数据手册以确认菊花链是否可用。

![](https://pic2.zhimg.com/v2-bfa6279e2a1707f1c28eac8bf02e3929_r.jpg)

**ADI 支持 SPI 的模拟开关与多路转换器**

ADI 公司最新一代支持 SPI 的开关可在不影响精密开关性能的情况下显著节省空间。本文的这一部分将讨论一个案例研究，说明支持 SPI 的开关或多路复用器如何能够大大简化系统级设计并减少所需的 GPIO 数量。

ADG1412 是一款四通道、单刀单掷 (SPST) 开关，需要四个 GPIO 连接到每个开关的控制输入。图 9 显示了微控制器和一个 ADG1412 之间的连接。

![](https://pic3.zhimg.com/v2-15dd6c3ef5f2175d8df0786c4fd6b182_r.jpg)

随着电路板上开关数量的增加，所需 GPIO 的数量也会显著增加。例如，当设计一个测试仪器系统时，会使用大量开关来增加系统中的通道数。在 4×4 交叉点矩阵配置中，使用四个 ADG1412。此系统需要 16 个 GPIO，限制了标准微控制器中的可用 GPIO。图 10 显示了使用微控制器的 16 个 GPIO 连接四个 ADG1412。

![](https://pic4.zhimg.com/v2-e4e2f20187c08d825d26df239a1df7ef_r.jpg)

  
**如何减少 GPIO 数量?**

一种方法是使用串行转并行转换器，如图 11 所示。该器件输出的并行信号可连接到开关控制输入，器件可通过串行接口 SPI 配置。此方法的缺点是外加器件会导致物料清单增加。  

![](https://pic4.zhimg.com/v2-7883ac06f7c1aa312958fcfefb9979c3_r.jpg)

另一种方法是使用 SPI 控制的开关。此方法的优点是可减少所需 GPIO 的数量，并且还能消除外加串行转并行转换器的开销。如图 12 所示，不需要 16 个微控制器 GPIO，只需要 7 个微控制器 GPIO 就可以向 4 个 ADGS1412 提供 SPI 信号。开关可采用菊花链配置，以进一步优化 GPIO 数量。在菊花链配置中，无论系统使用多少开关，都只使用主机 (微控制器) 的四个 GPIO。

![](https://pic3.zhimg.com/v2-8436db149fdb9af547d05ff25793a67e_r.jpg)

图 13 用于说明目的。ADGS1412 数据手册建议在 SDO 引脚上使用一个上拉电阻。为简单起见，此示例使用了四个开关。随着系统中开关数量的增加，电路板简单和节省空间的优点很重要。

![](https://pic2.zhimg.com/v2-3a8ad0e577c3a99f0092e630392ef985_b.jpg)

在 6 层电路板上放置 8 个四通道 SPST 开关，采用 4×8 交叉点配置时，ADI 公司支持 SPI 的开关可节省 20% 的总电路板空间。