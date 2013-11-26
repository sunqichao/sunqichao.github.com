---
layout: post
title: "学习cocos2d的第一天"
date: 2013-11-26 22:36:21 +0800
comments: true
categories: 
---

# 今天开始学习cocos2d

#### 首先是拿一个网上的demo来学习，修改自己的。没想到遇到的困难还真不少，api，方法名都不知道，很多都要到网上去查，下面纪录一下今天遇到的问题

### 做的是一个消除的小游戏

* 看到demo里面是画点的，而我想用图片，然后就不知道怎么替换图片了，靠。  网上找到方法，就是创建一个CCTexture2D然后替换:

代码:

	CCTexture2D * texture =[[CCTextureCache sharedTextureCache] addImage: @”pa_icon.png”];

	[sprite2 setTexture:texture];
	
	
* 想设置一个view的背景图片也找不到属性，然后又去网上找,在init方法的最上面加上此段代码，明白了cocos2d的坐标系是从左下角开始的

代码:
	
	CGSize winsize = [[CCDirector sharedDirector] winSize];
	CCSprite *back = [CCSprite spriteWithFile:@"Images/bgplaying.png"];
    back.position = ccp(winsize.width/2, winsize.height/2);
    [self addChild:back];

	
* 设置背景色为透明色:[super initWithColor:ccc4(255, 255, 255, 0)]-----最后一个参数改为0就透明了
* cocos2d里面有内置的画圆点的功能CCDrawNode，随着修改radius的值可以控制画出图形的形状

代码:

	    [m_selectNode drawDot:ccp(15, 15) radius:DRAWSPRITE_RADIUES color:col];

* 需要对一个Srpint进行缩小的操作;下面的使用是先创建两个缩放的动画，然后再创建一个执行队列，并且把动画放进去，最后让sprite执行这个动画队列。

代码:

	    CCScaleBy * scaleBy2 = [CCScaleBy actionWithDuration:0.2 scale:0];
  	    CCScaleBy * scaleBy2 = [CCScaleBy actionWithDuration:0.2 scale:0];
        CCSequence * seq = NULL;
        seq = [CCSequence actions:scaleBy,scaleBy2, nil];
        [itemSrpint runAction:seq];

* 设置sprtie的放大缩小比列的方法:    [itemSrpint setScale:1.0];




##今天学到的action的知识较多

>从sprite的原始点移动到指定的点

	    CCMoveTo * moveto = [CCMoveTo actionWithDuration:SPAWN_DROPDOWN_TIME/3 position:pos];


>使sprite从一个点开始跳跃，设置跳的高度和次数

	    CCJumpTo * jump = [CCJumpTo actionWithDuration:SPAWN_JUMP_TIME/3*2 position:pos height:20 jumps:1];

>设置动作执行完后的回调函数

	CCCallBlockO * callB = [CCCallBlockO actionWithBlock:^(id object) {
        m_hasSelected = NO;
        self.visible = YES;
    } object:self];
    
    
## 加载音频文件（最好是在初始化的时候就加载，用的时候播放加载好的音频文件就好了）

	[[SimpleAudioEngine sharedEngine] preloadEffect:@"Sounds/1.aif"];
    [[SimpleAudioEngine sharedEngine] playEffect:@"Sounds/1.aif"];


## 判断两种颜色是否相等

	ccc4FEqual(m_currentDrawColor, node.m_color)



## 感觉比较难点的技术就是画线的，但是看了代码后感觉条理还是很清晰的（我写的时候是把画线的方法写在draw方法里面，draw方法会在程序运行的时候一直调用，来刷新界面）

```
		//获取线的宽度
        glLineWidth(10);
    
        //设置线的颜色
        ccColor4B c4b = ccc4BFromccc4F(m_currentDrawColor);
        ccDrawColor4B(c4b.r, c4b.g, c4b.b, c4b.a);
        
        //根据两个点开始画线
        ccDrawLine(pos, pos1);

```









