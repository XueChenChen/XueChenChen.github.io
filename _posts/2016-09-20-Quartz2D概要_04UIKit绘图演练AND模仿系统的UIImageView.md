---
layout:     post
title:      "Quartz2D-UIKit绘图演练AND模仿系统的UIImageView"
date:       2016-09-21 20:30:00
author:     "Chen"
header-img: "img/bookNotes/quartz2D.png"
tags:
    - Quartz2D
    - Objective-C
    - 读书笔记
---

#
#05-UIKit绘图演练
>	一般使用UIKit给我们提供的绘图来绘制一些文字,图片这些东西.
>UIKit给我们提供画图的方法底层也是分为四步.
>所以也必须在drawRect方法当中去写.
**1.如何画文字?**
先创建好要画的文字
使用UIKit提供的方法进行绘制.
方法说明:
drawAtPoint:要画到哪个位置
withAttributes:文本的样式.
[str drawAtPoint:CGPointZero withAttributes:nil];
**2.如何添加绘制文字属性?**[文字属性大全][01]

通过绘制方法的最后一个属性withAttributes来设置文字属性.
它要求传入的是一个字典.它是通过字典的key和Value的形式来设置文字样式.
那传什么key,什么值我们可以在UIKit头文件当中的NSAttributedString类当中去找.
使用形式如下:
创建一个可变的字典,设置key,value
NSMutableDictionary *dict = [NSMutableDictionary dictionary];
字体
dict[NSFontAttributeName] = [UIFont systemFontOfSize:50];
颜色
dict[NSForegroundColorAttributeName] = [UIColor redColor];
设置边框颜色
dict[NSStrokeColorAttributeName] = [UIColor redColor];
dict[NSStrokeWidthAttributeName] = @1;
阴影
NSShadow *shadow = [[NSShadow alloc] init];
shadow.shadowOffset = CGSizeMake(10, 10);
shadow.shadowColor = [UIColor greenColor];
shadow.shadowBlurRadius = 3;
dict[NSShadowAttributeName] = shadow;
**3.drawAtPoint:和drawInRect:的区别?**

drawAtPoint:不能够自动换行
drawInRect:能够自动换行
**4.如果绘制图片?**

绘制图片同样开始要先把图片素材导入.
AtPoint:参数说明图片要绘制到哪个位置.
通过调用UIKit的方法drawAtPoint:CGPointZero方法进行绘制;
**5.在绘制图片过程当中.drawAtPoint:和drawInRect:两个方法的区别?**

drawAtPoint:绘制出来的图图片跟图片的实际尺寸一样大
drawInRect:使用这个方法绘制出来的图片尺寸会和传入的rect区域一样大.
**6.如果进行平铺图片?**

[image drawAsPatternInRect:rect];
**7.如何选用UIKit提供的方法快速画一个矩形?**

快速的用矩形去填充一个区域
UIRectFill(rect);
**8.如何利用UIKit裁剪一个区域?**

UIRectClip(CGRectMake(0, 0, 50, 50));
这个方法必须要设置好裁剪区域,才能有裁剪


[01]: ./NSAttributedSring的描述.md

#
#06-模仿系统的UIImageView

####整体思路:
我们想要模仿系统的UIImageView,我们必须得要知道系统的UIView怎么用.
系统的用法是创建一个UIImageView对象,设置frame,给它传递一个UIImage,再把它添加到一个View上面就可以了.
可以切换图片.
这是第一个用法.
第二种用法,就是在创建的时候直接传递一个UIImage对象,使用initWithImage的方法进行创建一个UImageView的方式
用这种做法创建出来的UIImageView它的尺寸大小和原始图片的尺寸大小一样大.
所以我们自己的UIImageView也要具有这些功能.
####实现步骤:
第一步:新建一个UIView,起名MyImageView.
第二步:给MyImageView添加一个UIImage属性,供外界传递图片
第三步:在DrawRect方法当中把传递的图片绘制到View上面
绘制方法为:[_image drawInRect:rect],绘制的图片尺寸大小和UIView的尺寸大小一样大.
第四步:重写UIImage属性的set方法,在set方法当中让View重新绘制.目的为了能够办到切换图片.
第五步:提供一个- (instancetype)initWithImage:(UIImage *)image方法.
在这个方法当中重写init方法
在初始化时,让View尺寸和图片的实际大小一样大.
然后再给UIImage属性赋值.
这样在绘制图片的时候,显示出来的View已经有尺寸了, 尺寸大小和图片的实际大小一样大.
具体代码实现:
- (instancetype)initWithImage:(UIImage *)image{
if (self = [super init]) {
self.frame = CGRectMake(0, 0, image.size.width, image.size.height);
_image = image;
}
return self;
}

-(void)setImage:(UIImage *)image{
_image = image;
[self setNeedsDisplay];
}
- (void)drawRect:(CGRect)rect {
[_image drawInRect:rect];
}