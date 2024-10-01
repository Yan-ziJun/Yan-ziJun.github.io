---
layout: post
title: 庆祝中华人民共和国成立75周年
date: 2024-10-01
Author: 阎子君
categories: China
tags: [中华人民共和国, The People's Republic of China]
comments: true
---

# 热烈庆祝中华人民共和国成立 75 周年！
## Warmly celebrate the 75th anniversary of the founding of the People's Republic of China.

**以下为使用vs + easyx 实现程序输出中华人民共和国国旗（非官方标准国旗排版与色号，仅为个人热爱祖国的一种表达方式）**

**具体代码如下：**

    #include  <stdio.h>
	#include  <easyx.h>
	#include <math.h>

	#define PI 3.14

	void fiveStar(int r, double startAngle)
	{
        double d = 2 * PI / 5;
        POINT points[5];
        for (int i = 0; i < 5; i++)
        {
            points[i].x = r * cos(startAngle + i * d * 2);
            points[i].y = r * sin(startAngle + i * d * 2);
        }
        solidpolygon(points, 5);
	}

	int main()
	{
        int width = 900, height = width / 3 * 2;
        int grid = width / 2 / 15;
        initgraph(width, height);
        setbkcolor(RED);
        cleardevice();

        setaspectratio(1, -1);
        setfillcolor(YELLOW);
        setpolyfillmode(WINDING);

        setorigin(grid * 5, grid * 5);
        fiveStar(grid * 3, PI / 2);

        setorigin(grid * 10, grid * 2);
        double startAngle = atan(3.0 / 5.0) + PI;
        fiveStar(grid, startAngle);

        setorigin(grid * 12, grid * 4);
        startAngle = atan(1.0 / 7.0) + PI;
        fiveStar(grid, startAngle);

        setorigin(grid * 12, grid * 7);
        startAngle = -atan(2.0 / 7.0) + PI;
        fiveStar(grid, startAngle);

        setorigin(grid * 10, grid * 9);
        startAngle = -atan(4.0 / 5.0) + PI;
        fiveStar(grid, startAngle);


        getchar();
        closegraph();
        return 0;

	}
	
**实现效果如下：**

<img src="/images/China/20241001China.png"/>

**文章内容版权归作者[阎子君](https://blog.zijun.us.kg/)所有，转载请与我联系获得授权许可**

