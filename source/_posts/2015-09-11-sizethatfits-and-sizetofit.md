---
layout: post
title: "sizeThatFits and sizeToFit"
date: 2015-09-11 17:36:30 +0800
comments: true
categories: iOS 转载
---
sizeThatFits and sizeToFit是UIView的两个方法, 官方文档上说： 

~~~
- (CGSize)sizeThatFits:(CGSize)size;
~~~

作用：return 'best' size to fit given size. does not actually resize view. Default is return existing view size

~~~
- (void)sizeToFit;
~~~

作用： calls sizeThatFits: with current view bounds and changes bounds size.

<!--more-->
example:

~~~
- (void)viewDidLoad
{
    [super viewDidLoad];
    UIView *view = [[UIView alloc] initWithFrame:CGRectMake(100, 100, 200, 100)];
    view.backgroundColor = [UIColor yellowColor];
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(5, 5, 0, 0)];
    [label setFont:[UIFont systemFontOfSize:20]];
    label.text = @"hello wdszgrf";
    CGSize sizeThatFits = [label sizeThatFits:CGSizeZero];
    NSLog(@"---- %f  %f ----", sizeThatFits.width, sizeThatFits.height);   
    // output:  ---- 117.000000  24.000000 ----

    NSLog(@"**** %f  %f ****", label.frame.size.width, label.frame.size.height);   
    // output:  **** 0.000000  0.000000 **** 说明sizeThatSize并没有改变原始label的大小
 
    [label sizeToFit];  // 这样搞就直接改变了这个label的宽和高，使它根据上面字符串的大小做合适的改变
    [label setCenter:CGPointMake(80, 50)];
    NSLog(@"==== %f %f ====", label.frame.size.width, label.frame.size.height);  	
    // output:   ==== 117.000000 24.000000 ==== 

    [view addSubview:label];
    [self.view addSubview:view];
}
~~~