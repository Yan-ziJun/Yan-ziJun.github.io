---
layout: post
title: 基于51单片机的循迹小车（未添加PID算法控制）
date: 2023-11-03
Author: 阎子君
categories: 51单片机
tags: [51单片机, 实践项目]
comments: true
---

**本文章内容为初学51单片机参加校内赛所作，本文仅代表个人想法和思路**

    我记得我是从大一上学期开始我的“造车史”的吧，当时是使用51单片机完成的人生的第一台智能车，在小组团队比赛中，我的小车的成绩还不错，哈哈哈，后面参加了学校组织的循迹小车竞赛，拿了个二等奖。

# 51单片机寻迹小车
## 工作原理

    比赛赛道为：白色底板加黑色中线
    
    简述我的小车工作流程：
    1、通过红外传感器模块收集小车处于赛道的位置，将信息传输给主控芯片。
    2、主控芯片将收集的信息与编写循迹程序内容比较，得出小车的驱动方向（前进、小角度左转、小角度右转、大角度左转、大角度右转），将信息输送给驱动模块（我使用的是L298N）。
    3、驱动模块通过收到的信息驱动四个轮子转动，调整驱动方向，按照规定路线行驶。

    出库：当时使用的方法是给主控写入死程序，按照固定的路线出库；另一种方法是感应到出库正对磁铁，运行出库函数（经过实践检验与入库控制冲突，不易控制，有很大的不确定性）。
    入库：行驶当中感应到车库门口的磁铁，执行入库函数。

## 硬件搭建

    由于复杂性不高，这部分就不详解了哈！（通过程序就可以直接搭建出来）。

## 程序代码

**以下为部分重要程序，并非完整程序**

**main.c**

    #include<reg51.h>    -> 必要头文件
    #include<intrins.h>  -> 延时函数需要调用的头文件
    #include<delay.h>    -> 存放调用的延时函数的头文件
    #include<hanshu.h>   -> 定义封装的基本函数的头文件
    #include<dingyi.h>   -> 定义端口的头文件
    #include<timer.h>    -> 存放定时器初始化/服务函数的头文件
    #include<xunji.h>    -> 定义轨迹驱动函数的头文件

    void main()
	{
		Timer0Init();  //定时器初始化
		chuku();       //出库
		while(1)
		{
			xunji();   //循迹
		}
	}

 **delay.h**
  
     void delay200ms(void)   
    {
        unsigned char a,b,c;
        for(c=67;c>0;c--)
            for(b=142;b>0;b--)
                for(a=9;a>0;a--);
    }
    
    void delay250ms(void)   
    {
        unsigned char a,b,c;
        for(c=5;c>0;c--)
            for(b=116;b>0;b--)
                for(a=214;a>0;a--);
    }
    
    void delay500ms(void)   
    {
        unsigned char a,b,c;
        for(c=205;c>0;c--)
            for(b=116;b>0;b--)
                for(a=9;a>0;a--);
    }

**dingyi.h**

    //定义左边两个电机 IN12控制左后轮（10前进 01后退 00停止）  IN43控制左前轮（10前进 01后退 00停止 ）
    sbit ENA_1 = P3^7;
    sbit IN1_1 = P3^6;
    sbit IN2_1 = P3^5;
    sbit IN3_1 = P3^4;
    sbit IN4_1 = P3^3;
    sbit ENB_1 = P3^2;
    //定义右边两个电机 IN12控制右前轮（10前进 01后退 00停止） IN43控制右后轮 （10前进 01后退 00停止）
    sbit ENA_2 = P1^0;
    sbit IN1_2 = P1^1;
    sbit IN2_2 = P1^2;
    sbit IN3_2 = P1^3;
    sbit IN4_2 = P1^4;
    sbit ENB_2 = P1^5;
    //定义循迹模块引脚
    sbit D0 = P2^5;
    sbit D1 = P2^3;
    sbit D2 = P2^2;
    sbit D3 = P2^1;
    sbit D4 = P2^0;
    sbit D6 = P2^6;
    //定义磁性开关引脚
    sbit D5 = P2^4;
    //电机占空比  左后   左前   右前   右后
    unsigned char PWMA_1,PWMB_1,PWMA_2,PWMB_2;
    //定义磁性开关感应到的次数
    int cishu = 0;

**hanshu.h**

    void xunji();     //循迹函数
    void straight();  //出入库直行函数
    void straight2(); //直行函数
    void stop();      //停止函数
    void turnleft();  //小角度左转函数（用于D2红外传感器压线）
    void turnleft2(); //大角度左转函数（用于D1红外传感器压线）
    void turnright(); //小角度右转函数（用于D3红外传感器压线）
    void turnright2();//大角度右转函数（用于D4红外传感器压线）
    void turnleft3(); //小角度左转函数（用于出库）
    //void turnleft4(); //大角度左转函数（用于十字路口）
    void turnleft5(); //大角度左转函数（用于入库）
    void turnleft6(); //超大角度左转函数
    void turnright6(); //超大角度右转函数
    void chuku();     //出库函数
    void ruku();      //入库函数
    
    void straight()   //出入库直行
    	{
    		IN1_1 = 1;
    		IN2_1 = 0;
    		PWMA_1 = 50;
    		IN4_1 = 1;
    		IN3_1 = 0;
    		PWMB_1 = 50;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 50;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 50;
    	}	
    	
    void straight2()   //直行
    	{
    		IN1_1 = 1;
    		IN2_1 = 0;
    		PWMA_1 = 50;
    		IN4_1 = 1;
    		IN3_1 = 0;
    		PWMB_1 = 50;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 50;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 50;
    	}	
    	
    void turnleft()   //小角度左转
    	{
    		IN1_1 = 0;
    		IN2_1 = 1;
    		PWMA_1 = 44;
    		IN4_1 = 0;
    		IN3_1 = 1;
    		PWMB_1 = 44;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 44;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 44;
    	}	
    	
    void turnright()   //小角度右转
    	{
    		IN1_1 = 1;
    		IN2_1 = 0;
    		PWMA_1 = 44;
    		IN4_1 = 1;
    		IN3_1 = 0;
    		PWMB_1 = 44;
    		IN1_2 = 0;
    		IN2_2 = 1;
    		PWMA_2 = 44;
    		IN4_2 = 0;
    		IN3_2 = 1;
    		PWMB_2 = 44;
    	}	
    
    void turnleft2()   //大角度左转
    	{
    		IN1_1 = 0;
    		IN2_1 = 1;
    		PWMA_1 = 70;
    		IN4_1 = 0;
    		IN3_1 = 1;
    		PWMB_1 = 70;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 70;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 70;
    	}
    
    void turnright2()   //大角度右转
    	{
    		IN1_1 = 1;
    		IN2_1 = 0;
    		PWMA_1 = 70;
    		IN4_1 = 1;
    		IN3_1 = 0;
    		PWMB_1 = 70;
    		
    		IN1_2 = 0;
    		IN2_2 = 1;
    		PWMA_2 = 70;
    		IN4_2 = 0;
    		IN3_2 = 1;
    		PWMB_2 = 70;
    	}
    	
    void stop()         //停车
    	{
    		IN1_1 = 0;
    		IN2_1 = 0;
    		PWMA_1 = 0;
    		IN4_1 = 0;
    		IN3_1 = 0;
    		PWMB_1 = 0;
    		
    		IN1_2 = 0;
    		IN2_2 = 0;
    		PWMA_2 = 0;
    		IN4_2 = 0;
    		IN3_2 = 0;
    		PWMB_2 = 0;
    	}	
    
    void turnleft3()    //出库左转
    	{
    		IN1_1 = 0;
    		IN2_1 = 1;
    		PWMA_1 = 50;
    		IN4_1 = 0;
    		IN3_1 = 1;
    		PWMB_1 = 50;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 50;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 50;
    	}
    
    /*void turnleft4()    //十字路口
    	{
    		IN1_1 = 0;
    		IN2_1 = 1;
    		PWMA_1 = 45;
    		IN4_1 = 0;
    		IN3_1 = 1;
    		PWMB_1 = 45;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 45;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 45;
    	}
    */
    void turnleft5()     //入库左转
    	{
    		IN1_1 = 0;
    		IN2_1 = 1;
    		PWMA_1 = 50;
    		IN4_1 = 0;
    		IN3_1 = 1;
    		PWMB_1 = 50;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 50;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 50;
    	}
    
    void turnleft6()     //防左转出
    	{
    		IN1_1 = 0;
    		IN2_1 = 1;
    		PWMA_1 = 85;
    		IN4_1 = 0;
    		IN3_1 = 1;
    		PWMB_1 = 85;
    		
    		IN1_2 = 1;
    		IN2_2 = 0;
    		PWMA_2 = 85;
    		IN4_2 = 1;
    		IN3_2 = 0;
    		PWMB_2 = 85;
    	}
    	
    void turnright6()     //防右转出
    	{
    		IN1_1 = 1;
    		IN2_1 = 0;
    		PWMA_1 = 85;
    		IN4_1 = 1;
    		IN3_1 = 0;
    		PWMB_1 = 85;
    		
    		IN1_2 = 0;
    		IN2_2 = 1;
    		PWMA_2 = 85;
    		IN4_2 = 0;
    		IN3_2 = 1;
    		PWMB_2 = 85;
    	}
    	
    void chuku()
    	{
    		delay250ms();
    		straight();
    		delay250ms();
    		turnleft3();
    		delay200ms();
    	}
    	
    void ruku()
    	{
    		turnleft5();
    		delay200ms();
    		straight();
    		delay250ms();
    		stop();
    		delay500ms();
    	}

**timer.h**

    void Timer0Init()
	{
		TMOD |= 0x01;              //定时器模式
		TL0    = (65536-100)%256;  //定时初始值
		TH0    = (65536-100)/256;  //定时初始值
		TR0    = 1;                //定时器0开始计时
		ET0    = 1;                //定时器0中断使能
		EA     = 1;                //中断使能
	}
	
    void T0isp() interrupt 1
    {
    	unsigned char a,b,c,d;
    	TL0 = (65536-100)%256;
    	TH0 = (65536-100)/256;
    	a++;
    	b++;
    	c++;
    	d++;
    		
    	if(a<PWMA_1)
    	{
    		ENA_1 = 1;
    	}
    	else
    	{
    		ENA_1 = 0;
    		if(a>=100)
    		{
    			a = 0;
    		}
    	}
    		
    	if(b<PWMB_1)
    	{
    		ENB_1 = 1;
    	}
    	else
    	{
    		ENB_1 = 0;
    		if(b>=100)
    		{
    			b = 0;
    		}
    	}
    		
    	if(c<PWMA_2)
    	{
    		ENA_2 = 1;
    	}
    	else
    	{
    		ENA_2 = 0;
    		if(c>=100)
    		{
    			c = 0;
    		}
    	}
    		
    	if(d<PWMB_2)
    	{
    		ENB_2 = 1;
    	}
    	else
    	{
    		ENB_2 = 0;
    		if(d>=100)
    		{
    			d = 0;
    		}
    	}
    }

**xunji.h**

    void xunji()
	{
		if(D0 == 0 && D1 == 0 && D2 == 0 && D3 == 0 && D4 == 0 && D6 == 0)         //0 0 0 0 0 0 直行
		{
			straight2();
		}
		
		else if(D0 == 0 && D1 == 0 && D2 == 1 && D3 == 0 && D4 == 0 && D6 == 0)    //0 0 1 0 0 0 小角度左转
		{
			turnleft();
		}
		
		else if(D0 == 0 && D1 == 0 && D2 == 0 && D3 == 1 && D4 == 0 && D6 == 0)    	//0 0 0 1 0 0 小角度右转
		{
			turnright();
		}
		
		else if(D0 == 0 && D1 == 1 && D2 == 0 && D3 == 0 && D4 == 0 && D6 == 0)    	//0 1 0 0 0 0 大角度左转
		{
			turnleft2();
		}
		
		else if(D0 == 0 && D1 == 1 && D2 == 1 && D3 == 0 && D4 == 0 && D6 == 0)    	//0 1 1 0 0 0 大角度左转
		{
			turnleft2();
		}
		
		else if(D0 == 0 && D1 == 0 && D2 == 0 && D3 == 0 && D4 == 1 && D6 == 0)   	//0 0 0 0 1 0 大角度右转
		{
			turnright2();
		}
		
		else if(D0 == 0 && D1 == 0 && D2 == 0 && D3 == 1 && D4 == 1 && D6 == 0)         //0 0 0 1 1 0 大角度右转
		{
			turnright2();
		}
		
		else if(D0 == 1 && D1 == 0 && D2 == 0 && D3 == 0 && D4 == 0 && D6 == 0)         //1 0 0 0 0 0 超大角度左转
		{
			turnleft6();
		}
		
		else if(D0 == 1 && D1 == 1 && D2 == 0 && D3 == 0 && D4 == 0 && D6 == 0)         //1 1 0 0 0 0 超大角度左转
		{
			turnleft6();
		}
		
		else if(D0 == 0 && D1 == 0 && D2 == 0 && D3 == 0 && D4 == 0 && D6 == 1)         //0 0 0 0 0 1 超大角度右转
		{
			turnright6();
		}
		
		else if(D0 == 0 && D1 == 0 && D2 == 0 && D3 == 0 && D4 == 1 && D6 == 1)         //0 0 0 0 1 1 超大角度右转
		{
			turnright6();
		}
		
		else if(D0 == 1 && D1 == 1 && D2 == 1 && D3 == 1 && D4 == 1 && D6 == 1)         //1 1 1 1 1 1 停止
		{
			stop();
		}
		else if(D5 == 0)
		{
			ruku();
		}
	}

## 项目中反思与建议

    首先，本人负责该项目的软件编写与调试工作，做完此项目回首看来，程序编写存在严重的不规范性和不可读性。以下是程序编写时的一些建议：
    
    1、各模块封装的头文件调用问题，各模块封装函数应当在此模块的源文件中进行定义，然后在头文件中列出各封装函数供主函数调用。（这个问题可以从上述程序中轻易看出）
    2、无论是封装函数还是变量定义，都需要（按照国际惯例哈）使用可以体现其含义和作用的英文单词，而不是拼音。增强程序的可读性（显的高大尚些）。

    其次，在程序调试工作中，我的一些建议：

    1、在打板前一定要和负责硬件的伙伴讲清楚（甚至要求，如果自己完成一个项目需要更加清楚）要将单片机程序下载端口那一组单独引出来，避免调试时频繁的插线拔线，可以极大的提高程序调试效率。（负责硬件的应该都清楚，当时本人可能刚学习单片机不太了解，才有后边换来的教训）
    2、针对此项目：调试路线：小车不可以大幅度摇摆（最好沿线平缓运行） -> 能够完成基本的赛事要求 -> 最后再争取最短完赛时间 （可以添加 PID 算法加持，使运行过程更加丝滑更加流畅）

**文章内容版权归作者[阎子君](https://blog.zijun.us.kg/)所有，转载请与我联系获得授权许可**

