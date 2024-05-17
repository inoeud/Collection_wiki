> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/m0_58168670/article/details/130588514)

除非特别说明 -> 本部分适用于整个 STM32F4xx 系列。

**spi 概论**：

         它可用于多种用途，包括基于双线的单工同步传输，其中一条可作为双向数据线，或使用 CRC 校验实现可靠通信, 它可在全双工模式（使用 4 引脚）或半双工模式（使用 3 个引脚）下作为从器件或主器件工作.

**SPI 特性（特点）：**

● 基于四条线的全双工同步传输  
● 基于双线的单工同步传输，其中一条可作为双向数据线  
● 8 位或 16 位传输帧格式选择  
● 主模式或从模式操作  
● 多主模式功能  
● 8 个主模式波特率预分频器（最大值为 f PCLK /2）  
● 从模式频率（最大值为 f PCLK /2）  
● 对于主模式和从模式都可实现更快的通信  
● 对于主模式和从模式都可通过硬件或软件进行 NSS 管理：动态切换主 / 从操作  
● 可编程的时钟极性和相位  
● 可编程的数据顺序，最先移位 MSB 或 LSB  
● 可触发中断的专用发送和接收标志  
● SPI 总线忙状态标志  
● SPI TI 模式  
● 用于确保可靠通信的硬件 CRC 功能：  
— 在发送模式下可将 CRC 值作为最后一个字节发送  
— 根据收到的最后一个字节自动进行 CRC 错误校验  
● 可触发中断的主模式故障、上溢和 CRC 错误标志  
● 具有 DMA 功能的 1 字节发送和接收缓冲器：发送和接收请求

**SPI 通过 4 个引脚与外部器件连接：**  
**● MISO：**主输入 / 从输出数据。此引脚可用于在从模式下发送数据和在主模式下接收数据。  
**● MOSI：**主输出 / 从输入数据。此引脚可用于在主模式下发送数据和在从模式下接收数据。  
**● SCK：**用于 SPI 主器件的串行时钟输出以及 SPI 从器件的串行时钟输入。  
**● NSS：**从器件选择。这是用于选择从器件的可选引脚。此引脚用作 “**片选**”，可让 SPI 主器件与从器件进行单独通信，从而并避免数据线上的竞争。从器件的 NSS 输入可由主器件上的标准 IO 端口驱动。NSS 引脚在使能（SSOE 位）时还可用作输出，并可在 SPI 处于主模式配置时驱动为低电平。通过这种方式，只要器件配置成 NSS 硬件管理模式，所有连接到该主器件 NSS 引脚的其它器件 NSS 引脚都将呈现低电平，并因此而作为从器件。当配置为主模式，且 NSS 配置为输入（MSTR=1 且 SSOE=0）时，如果 NSS 拉至低电平，SPI 将进入主模式故障状态：MSTR 位自动清零，并且器件配置为从模式

 **SPI“一对一” 基本示例：**

  MOSI 引脚连接在一起，MISO 引脚连接在一起。通过这种方式，主器件和从器件之间以串行方式传输数据（最高有效位在前）。

              **特别注意**：**通信始终由主器件发起。**当主器件通过 MOSI 引脚向从器件发送数据时，从器件 MISO 引脚做出响应。这是一个数据输出和数据输入都由同一时钟进行同步的 1 全双工通信同时通过过程  
![](https://img-blog.csdnimg.cn/20b8402446724ca89267d53d72601c7a.png)

  **SPI“一对多” 基本示例：**

 主机通过控制 css 引脚，对于从机的选择，如果设计 css 引脚拉低，从而从机的引脚也检测到低频信号，说明自己被选中，搜索 ss1,ss2 ss3, 几个片选引脚应该是互斥的，同一时刻，只有一个电平是低的.

**![](https://img-blog.csdnimg.cn/4ae1a5ef63db49d4a8dcc63263a78e8d.png)**

   **SPI“如何通过 css 引脚对从机的选择：**

● 软件管理 NSS (SSM = 1)--->**（常用方式）**  
● 硬件管理 NSS (SSM = 0)

 **时钟相位和时钟极性：（决定了四种模式）**

通过 SPI_CR1 寄存器中的 CPOL 和 CPHA 位，可以用软件选择四种可能的时序关系。

  
**CPOL（时钟极性）**位控制不传任何数据时的时钟电平状态。此位对主器件和从 器件都有作  
用。如果复位 CPOL，SCK 引脚在空闲状态处于低电平。如果将 CPOL 置 1，SCK 引脚在  
空闲状态处于高电平。

 ![](https://img-blog.csdnimg.cn/52a30663d55f49148c525a8c706bc378.png)

如果将 **CPHA（时钟相位）**位置 1，则 SCK 引脚上的第二个边沿（如果复位 CPOL 位，则为下降沿；如果将 CPOL 位置 1，则为上升沿）对 MSBit 采样。即，在第二个时钟边沿锁存数据。如果复位 CPHA 位，则 SCK 引脚上的第一个边沿（如果将 CPOL 位置 1，则为下降  
沿；如果复位 CPOL 位，则为上升沿）对 MSBit 采样。即，在第一个时钟边沿锁存数据。

![](https://img-blog.csdnimg.cn/fd9dea9581b54a03b846a3dda8f3300f.png)

 **四种模式：（其中模式 0 和模式三较为常用）**

**![](https://img-blog.csdnimg.cn/fe47bb08deea464f906716d292218845.png)**

**四种模式的波形图示 ：**![](https://img-blog.csdnimg.cn/1ade98bd4aa4496e95c97cc5874f92f4.png)

**数据帧格式：**

                 主机传输一位一位的传输，低位传输还是高位传输看从机。

移出数据时 MSB （高位）在前还是 LSB（低位） 在前取决于 SPI_CR1 寄存器中 LSBFIRST 位的值。每个数据帧的长度均为 8 位或 16 位，具体取决于使用 SPI_CR1 寄存器中的 DFF 位。所选  
的数据帧格式适用于发送和 / 或接收。

![](https://img-blog.csdnimg.cn/7e390452a2d141d6b593786f5de13f94.png)

**SPI 寄存器的介绍：** 

![](https://img-blog.csdnimg.cn/0b1cc81313a4460b8126260f8a397f29.png)

**SPI 总线 stm32 代码配置的方式：**

```
	GPIO_InitTypeDef GPIO_InitStructure;
	SPI_InitTypeDef  SPI_InitStructure;
	
	//1.打开SPI1的外设时钟 + GPIO外设时钟
	RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOB, ENABLE);
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_SPI1, ENABLE);
	
	//2.配置GPIO引脚
	GPIO_InitStructure.GPIO_Pin 	= GPIO_Pin_3|GPIO_Pin_4|GPIO_Pin_5;	//引脚编号
	GPIO_InitStructure.GPIO_Mode 	= GPIO_Mode_AF;				//复用模式
	GPIO_InitStructure.GPIO_PuPd 	= GPIO_PuPd_NOPULL;			//无上下拉
	GPIO_Init(GPIOB, &GPIO_InitStructure);
 
	GPIO_InitStructure.GPIO_Pin 	= GPIO_Pin_14;			//引脚编号
	GPIO_InitStructure.GPIO_Mode 	= GPIO_Mode_OUT;		//输出模式
	GPIO_InitStructure.GPIO_PuPd 	= GPIO_PuPd_NOPULL;		//无上下拉
	GPIO_Init(GPIOB, &GPIO_InitStructure);
 
	//3.空闲状态下CS片选引脚默认高电平 表示不通信
	W25Q128_CS(1);
	
	//4.把GPIO引脚的功能复用为SPI1
	GPIO_PinAFConfig(GPIOB,GPIO_PinSource3, GPIO_AF_SPI1);
	GPIO_PinAFConfig(GPIOB,GPIO_PinSource4, GPIO_AF_SPI1);
	GPIO_PinAFConfig(GPIOB,GPIO_PinSource5, GPIO_AF_SPI1);
	
	//5.配置SPI1外设参数
	SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;	//全双工通信
	SPI_InitStructure.SPI_Mode = SPI_Mode_Master;			//主机模式
	SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;		//数据位数8bit
	SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;				//时钟极性高
	SPI_InitStructure.SPI_CPHA = SPI_CPHA_2Edge;			//第二个边沿采集数据
	SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;				//软件控制CS
	SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_16;	//84MHZ/16 = 5.25Mbps W25Q128支持35Mbps
 
	SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;		//高位先出
	SPI_Init(SPI1, &SPI_InitStructure);
	
	//6.使能SPI1外设
	SPI_Cmd(SPI1, ENABLE);
```