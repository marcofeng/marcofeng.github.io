---
layout: post
title: "设置返回按钮"
date: 2015-09-18 11:08:32 +0800
comments: true
categories: iOS 原创
---

设置导航栏返回按钮的方法:
导航栏的返回按钮backBarButtonItem需要设置在前一个页面，属于上一层的navigationItem。

 e.g.

 ~~~

- (void)setBackButtonTitle:(NSString *)title
{
    if (self.navigationController == nil || title.length == 0) {
        return;
    }

    UIBarButtonItem *barButton = [[UIBarButtonItem alloc] init];
    barButton.title = title;
    UIColor *titleColor = [self _getButtonTextColor];
    [barButton setTitleTextAttributes:@{NSForegroundColorAttributeName: titleColor} forState:UIControlStateNormal];
    //设置字体颜色

    if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"7.1")) {
        UIImage *backButtonImg = [[self _getBackImage] resizableImageWithCapInsets:UIEdgeInsetsMake(1.f, 7.f, 1.f, 1.f)];
        [barButton setBackButtonBackgroundImage:backButtonImg forState:UIControlStateNormal barMetrics:UIBarMetricsDefault];
        //设置按钮背景图（“<”），内间距
    }
    
    // backBarButtonItem需要设置在前一个页面！！！
    NSArray *viewControllers = self.navigationController.viewControllers;
    if (viewControllers.count < 2) {
        UIViewController *vc = viewControllers[0];
        vc.navigationItem.backBarButtonItem = barButton;
    } else {
        UIViewController *vc = viewControllers[viewControllers.count-2];
        vc.navigationItem.backBarButtonItem = barButton;
    }
}

 ~~~