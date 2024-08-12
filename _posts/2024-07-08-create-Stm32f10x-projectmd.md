---
layout: post
title: keil+stm32f10x标准库建立项目工程
date: 2024-07-08
Author: 阎子君
categories: stm32f10x
tags: [keil, stm32]
comments: true
--- 

**注意：本文章仅为个人学习方便编辑，属于个人习惯，并非官方标准库建立工程流程**

## 建立工程并配置工程步骤

#### 新建一个文件夹作为工程文件夹，此文件夹路径最好是英文。

#### 打开keil5_MDK软件，开始新建工程并配置工程。

    Project -> New pVision Project -> 选择工程文件夹(个人习惯放在User文件夹中)并起工程名 -> 选择芯片型号STM32F103C8（以Stm32f103c8t6为例）
    
**在文件夹中新建一下文件夹：**

    Start   -> 用于存放启动文件
    Library -> 用于存放标准库文件
    System  -> 用于存放标准外设库文件和自建外设库文件
    Output  -> 用于存放工程输出文件（可执行文件.hex文件等）
    User    -> 用于存放main文件等重要文件

**在keil工程中将Start、Library、System、User文件夹路径添加进来。(小锤子 -> C/C++ -> Include Paths)**
    
**顺便在 小锤子 -> C/C++ -> Define 中添加重要语句 USE_STDPERIPH_DRIVER**

**打开官方标准库文件夹，将需要的文件复制到工程文件夹中：**

          <- STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\CoreSupport中的两个文件(.c/.h)
    Start <- STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x中的三个文件(.h/.c/.h)
          <- STM32F10x\STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x\startup\arm中的全部文件

    Library <- STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\STM32F10x_StdPeriph_Driver\src中的全部源文件
            <- STM32F10x_StdPeriph_Lib_V3.6.0\Libraries\STM32F10x_StdPeriph_Driver\inc中全部的头文件

    User <- STM32F10x_StdPeriph_Lib_V3.6.0\Project\STM32F10x_StdPeriph_Template中的四个文件(.c/_it.c/_it.h/.h)

    Output <- 改变输出路径 小锤子 -> ouput -> select folder for objects -> 选择Output文件夹
    
**在keil工程中相应文件夹中添加这些文件(注意：启动文件夹Start中只能选择一个.s文件启动)，选择规则如下：**
    
    LD_VL  小容量产品超值系列  16-32K(Flash容量) STM32F100
    MD_VL  中容量产品超值系列  64-128K          STM32F100
    HD_VL  大容量产品超值系列  256-512K         STM32F100
    LD     小容量产品         16-32K           STM32F101/102/103
    MD     中容量产品         64-128K          STM32F101/102/103
    HD     大容量产品         256-512K         STM32F101/102/103
    XL     超大容量产品       大于512K         STM32F101/102/103
    CL     互联型产品                          STM32F105/107









**文章内容版权归作者[阎子君](https://blog.zijun.us.kg/)所有，转载请与我联系获得授权许可**

