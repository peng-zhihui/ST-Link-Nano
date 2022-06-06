## 前言

某宝上的STLINK V2下载器偶尔会坏掉，我们尝试修复一下

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507112035111-678843225.png)

## 1.材料

（1）完好的STLINK V2下载器和坏掉的下载器各1个；

（2）固件：https://gitee.com/Cai-Zi/stm32f103c8t6_dap_swo，也可以使用蓝色板制作哦

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507112301016-77428767.png)

## 2.硬件

### 2.1原理图

此下载器的2x5P接口中，SWD接口为：**SWDIO-PB14，SWCLK-PB13**

> 笔者的下载器主控芯片是64Pin，无法烧录固件，猜测是芯片挂了，于是找了片**STM32F103R8T6**焊接了上去

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507112518566-1063898676.png)

### 2.2固件引脚说明

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507112459498-323949034.png)

## 3.烧录固件

将坏掉的下载器、完好的下载器和电脑连接好；

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507112035111-678843225.png)

打开STM32 ST-LINK Utility，进行连接；

 ![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202009/2116918-20200918223626169-152013751.png)

点击Target》Program...，找到下载好的[F103-DAP-SWO-CDC-STLINK_V20-SWO_PA10.hex](https://gitee.com/Cai-Zi/stm32f103c8t6_dap_swo/blob/master/Hex/F103-DAP-SWO-CDC-STLINK_V20-SWO_PA10.hex)，烧录即可。

![img](https://www.icode9.com/i/ll/?i=20200730223520867.png?,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjI2ODA1NA==,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 4.驱动配置

下载[UsbDriverTool](https://visualgdb.com/UsbDriverTool/)，如图安装WinUSB驱动

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507113252282-1443358078.png)

安装好后，设备管理器出现3种设备，Done！

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507113317117-796835457.png)

## 5.使用DAPLink调试

keil工程里，魔术棒设置如下
![图片](https://gitee.com/Cai-Zi/stm32f103c8t6_dap_swo/raw/master/Doc/Bluepill/OptionsforTarget.png)

勾选SWJ，Port选择SW，Connect选择Normal，Reset选择SYSRESETREQ
![图片](https://gitee.com/Cai-Zi/stm32f103c8t6_dap_swo/raw/master/Doc/Bluepill/SWDIO.png)

## 6.关于ST-LINK

参考[ST官方文档](https://www.st.com/content/ccc/resource/technical/document/technical_note/group0/30/c8/1d/0f/15/62/46/ef/DM00290229/files/DM00290229.pdf/jcr:content/translations/en.DM00290229.pdf)，官方推出了三大版本：V1、V2和V3

> 几个ST-LINK共存版本是随着时间的推移不断增加新功能的结果，
> 从第一个ST-LINK/V1版本开始。本节简要介绍了版本命名。
> ST-LINK的前两个版本都是独立的，并且嵌入了STMicroelectronics Discovery和Eval开发板。这些版本是：
> •ST-LINK/V1（现已过时）
> •ST-LINK/V2
>
> 第三个ST-LINK版本，ST-LINK/V2-1，是ST-LINK/V2的一个改进，增加了USB接口（存储接口和虚拟COM端口）以及更好的电源管理控制
> 申请委员会。ST-LINK/V2-1部署在最近的STMicroelectronics Discovery、Eval和Nucleo开发板。
> 另外两个版本是从ST-LINK/V2版本派生的，为了支持ST-LINK/V2-1的一些功能：
>
> •ST-LINK/V2-A，用于大容量存储
> •ST-LINK/V2-B，用于大容量存储和虚拟COM端口
> STLINK-V3是最新和最强大的ST-LINK代。它首先作为一个模块化的单机版引入探针（STLINK-V3SET）被改编成更紧凑的衍生物（STLINK-V3MINI和STLINK-V3MODS），可能也可嵌入演示板（STLINK-V3E）。STLINK-V3具有专门开发的多路径USB网桥功能。
> 各种ST-LINK实现嵌入了基于Arm® Cortex®‑M的STM32位微控制器。

![img](https://www.icode9.com/i/l/?n=20&i=blog/2116918/202105/2116918-20210507115126939-1356504231.png)

- ST-Link/V2：支持STM32和STM8调试，不带虚拟串口，TB上卖的大多是这种，目前手头还有好几个这个版本的ST-Link。后面会使用这个版本进行烧录。
- ST-LinkV2-1: 支持STM32调试，带虚拟串口和虚拟U盘下载，目前ST官方的Nucleo系列评估板上面板载的ST-Link就是这个版本。

## 参考链接

- [STM32F103C8T6_CMSIS-DAP_SWO](https://github.com/RadioOperator/STM32F103C8T6_CMSIS-DAP_SWO)
- [CMSIS-DAP](https://github.com/x893/CMSIS-DAP)
- [nanoDAP](https://github.com/wuxx/nanoDAP)
- [ST_LINK-V2_1](https://www.oshwhub.com/CYIIOT/ST_LINK-V2_1)