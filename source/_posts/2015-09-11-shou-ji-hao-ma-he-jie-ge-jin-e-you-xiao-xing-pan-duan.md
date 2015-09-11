---
layout: post
title: "手机号码和价格金额有效性判断"
date: 2015-09-11 16:29:32 +0800
comments: true
categories: 
---
在实际开发过程中，经常会遇到些不能让用户随便地输入手机号码，对输入的手机号码的正确判断；有些输入框只能输入数字，不能输入字母或特殊字符；还有些如价格金额之类的就只能输入数字和小数点且小数点后面保留两位。

<!--more-->
~~~
// 手机号码的有效性判断  
  
- (BOOL)isMobileNumber:(NSString *)mobileNum  
{  
    /**  
     * 手机号码  
     * 移动：134[0-8],135,136,137,138,139,150,151,157,158,159,182,187,188  
     * 联通：130,131,132,152,155,156,185,186  
     * 电信：133,1349,153,180,189  
     */  
    NSString * MOBILE = @"^1(3[0-9]|5[0-35-9]|8[025-9])\\d{8}$";  
    /**  
     10         * 中国移动：China Mobile  
     11         * 134[0-8],135,136,137,138,139,150,151,157,158,159,182,187,188  
     12         */  
    NSString * CM = @"^1(34[0-8]|(3[5-9]|5[017-9]|8[278])\\d)\\d{7}$";  
    /**  
     15         * 中国联通：China Unicom  
     16         * 130,131,132,152,155,156,185,186  
     17         */  
    NSString * CU = @"^1(3[0-2]|5[256]|8[56])\\d{8}$";  
    /**  
     20         * 中国电信：China Telecom  
     21         * 133,1349,153,180,189  
     22         */  
    NSString * CT = @"^1((33|53|8[09])[0-9]|349)\\d{7}$";  
    /**  
     25         * 大陆地区固话及小灵通  
     26         * 区号：010,020,021,022,023,024,025,027,028,029  
     27         * 号码：七位或八位  
     28         */  
    // NSString * PHS = @"^0(10|2[0-5789]|\\d{3})\\d{7,8}$";  
    /**  
     29         * 国际长途中国区(+86)  
     30         * 区号：+86  
     31         * 号码：十一位  
     32         */  
     NSString * IPH = @"^\\+861(3|5|8)\\d{9}$";  
      
    NSPredicate *regextestmobile = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", MOBILE];  
    NSPredicate *regextestcm = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CM];  
    NSPredicate *regextestcu = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CU];  
    NSPredicate *regextestct = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", CT];  
    NSPredicate *regextestiph = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", IPH];  
    if (([regextestmobile evaluateWithObject:mobileNum] == YES)  
        || ([regextestcm evaluateWithObject:mobileNum] == YES)  
        || ([regextestct evaluateWithObject:mobileNum] == YES)  
        || ([regextestcu evaluateWithObject:mobileNum] == YES)  
        || ([regextestiph evaluateWithObject:mobileNum] == YES))  
    {  
        return YES;  
    }  
    else  
    {  
        return NO;  
    }  
}  

//////// 特殊字符的限制输入，价格金额的有效性判断  
#define myDotNumbers     @"0123456789.\n"  
#define myNumbers        @"+0123456789\n"  
  
-(void) createTextFiled {  
      
    textfield1_ = [[UITextField alloc] initWithFrame:CGRectMake(0, 0, 110, 20)];  
    textfield1_.delegate = self;  
    textfield1_.text=@"";  
    textfield1_.keyboardType=UIKeyboardTypePhonePad;  
    textfield1_.textColor=[UIColor redColor];  
    textfield1_.backgroundColor=[UIColor whiteColor];  
    [self.view addSubview:textfield1_];  
    [textfield1_ release];  
      
    textfield2_ = [[UITextField alloc] initWithFrame:CGRectMake(110, 0, 80, 20)];  
    textfield2_.delegate = self;  
    textfield2_.text=@"11";  
    [self.view addSubview:textfield2_];  
    [textfield2_ release];  
      
    textfield3_ = [[UITextField alloc] initWithFrame:CGRectMake(200, 0, 100, 20)];  
    textfield3_.delegate = self;  
    textfield3_.text=@"22";  
    [self.view addSubview:textfield3_];  
    [textfield3_ release];  
}  
  
-(void)showMyMessage:(NSString*)aInfo {  
      
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"提示"  
                                                        message:aInfo  
                                                       delegate:self  
                                              cancelButtonTitle:@"确定"  
                                              otherButtonTitles:nil, nil];  
    [alertView show];  
    [alertView release];  
}  
  
  
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string {  
    NSCharacterSet *cs;  
    if ([textField isEqual:textfield1_]) {  
        cs = [[NSCharacterSet characterSetWithCharactersInString:myNumbers] invertedSet];  
        NSString *filtered = [[string componentsSeparatedByCharactersInSet:cs] componentsJoinedByString:@""];  
        BOOL basicTest = [string isEqualToString:filtered];  
        if (!basicTest) {  
            [self showMyMessage:@"只能输入数字"];  
            return NO;  
        }  
    }  
    else if ([textField isEqual:textfield2_]) {  
        cs = [[NSCharacterSet characterSetWithCharactersInString:myNumbers] invertedSet];  
        NSString *filtered = [[string componentsSeparatedByCharactersInSet:cs] componentsJoinedByString:@""];  
        BOOL basicTest = [string isEqualToString:filtered];  
        if (!basicTest) {  
            [self showMyMessage:@"只能输入数字"];  
            return NO;  
        }  
    }  
    else if ([textField isEqual:textfield3_]) {  
        NSUInteger nDotLoc = [textField.text rangeOfString:@"."].location;  
        if (NSNotFound == nDotLoc && 0 != range.location) {  
            cs = [[NSCharacterSet characterSetWithCharactersInString:myDotNumbers] invertedSet];  
        }  
        else {  
            cs = [[NSCharacterSet characterSetWithCharactersInString:myNumbers] invertedSet];  
        }  
        NSString *filtered = [[string componentsSeparatedByCharactersInSet:cs] componentsJoinedByString:@""];  
        BOOL basicTest = [string isEqualToString:filtered];  
        if (!basicTest) {  
              
            [self showMyMessage:@"只能输入数字和小数点"];  
            return NO;  
        }  
        if (NSNotFound != nDotLoc && range.location > nDotLoc + 3) {  
            [self showMyMessage:@"小数点后最多三位"];  
            return NO;  
        }  
    }  
    return YES;  
}
~~~
