//
//  LoginCtr.m
//  CMdriverPro
//
//  Created by zhuwei on 14-9-11.
//  Copyright (c) 2014年 zhuwei. All rights reserved.
//

#import "LoginCtr.h"
#import "BDRequestData.h"
#import "Utility.h"
#import "BDPersonCtr.h"
#import "BDRegisterCtr.h"

#define  USerNameTxt    (@"USerNameTxt")
@interface LoginCtr ()<UITextFieldDelegate>{
    UITextField * userNameText;
    UITextField * passwdText;
    
    UIButton * loginAction;
    UIButton * registerAction;
    BDRequestData * loginrequest;
}

@end

@implementation LoginCtr

- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
{
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        // Custom initialization
    }
    return self;
}
- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];
    self.navigationController.navigationBarHidden = YES;
}
- (void)viewDidLoad
{
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    self.view.backgroundColor = [UIColor clearColor];
    
    UIImageView * backView = [[UIImageView alloc] initWithFrame:self.view.bounds];
    [backView setImage:[UIImage imageNamed:@"backgroup"]];
    [self.view addSubview:backView];
    
    UIImage * img = [UIImage imageNamed:@"login_title"];
    UIImageView * loginTitleView = [[UIImageView alloc] initWithFrame:CGRectMake(53, 120, img.size.width, img.size.height)];
    loginTitleView.backgroundColor = [UIColor clearColor];
    [loginTitleView setImage:img];
    
    [self.view addSubview:loginTitleView];
    
    img = [UIImage imageNamed:@"login_edittext"];
    UIImageView * loginInputView = [[UIImageView alloc] initWithFrame:CGRectMake(33, CGRectGetMaxY(loginTitleView.frame)+ 45, img.size.width, img.size.height)];
    
    [loginInputView setBackgroundColor:[UIColor clearColor]];
    [loginInputView setImage:img];
    loginInputView.userInteractionEnabled = YES;
    
    userNameText = [[UITextField alloc] initWithFrame:CGRectMake(40, 1, 176, 35)];
    [userNameText setBackgroundColor:[UIColor clearColor]];
    NSString * userName = [[NSUserDefaults standardUserDefaults] objectForKey:USerNameTxt];
    if (userName) {
        [userNameText setText:userName];
    }
    [userNameText setDelegate:self];
    [userNameText setReturnKeyType:UIReturnKeyNext];
    [loginInputView addSubview:userNameText];
    
    passwdText = [[UITextField alloc] initWithFrame:CGRectMake(userNameText.frame.origin.x, CGRectGetMaxY(userNameText.frame)+2, CGRectGetWidth(userNameText.frame), CGRectGetHeight(userNameText.frame))];
    [passwdText setBackgroundColor:[UIColor clearColor]];
    [passwdText setDelegate:self];
    [passwdText setClearsContextBeforeDrawing:YES];
    [passwdText setSecureTextEntry:YES];
    [passwdText setReturnKeyType:UIReturnKeyDefault];
    [loginInputView addSubview:passwdText];
    
    [self.view addSubview:loginInputView];
    
    img = [UIImage imageNamed:@"login_btn"];
    loginAction = [UIButton buttonWithType:UIButtonTypeCustom];
    [loginAction setFrame: CGRectMake(loginInputView.frame.origin.x, CGRectGetMaxY(loginInputView.frame) + 15, img.size.width, img.size.height)];
    [loginAction setBackgroundImage:img
                           forState:UIControlStateNormal];
    [loginAction addTarget:self
                    action:@selector(clickButtonAction:)
          forControlEvents:UIControlEventTouchUpInside];
    
    [self.view addSubview:loginAction];
    
    img = [UIImage imageNamed:@"login_regbtn"];
    registerAction = [UIButton buttonWithType:UIButtonTypeCustom];
    [registerAction setFrame: CGRectMake(CGRectGetMaxX(loginAction.frame) + 8,loginAction.frame.origin.y, img.size.width, img.size.height)];
    [registerAction setBackgroundImage:img
                           forState:UIControlStateNormal];
    [registerAction addTarget:self
                    action:@selector(clickButtonAction:)
          forControlEvents:UIControlEventTouchUpInside];

    [self.view addSubview:registerAction];

    
}

- (void)clickButtonAction:(UIButton *)button {
    
    if ([button isEqual:loginAction]) {
        //登录操作
        if([userNameText.text length] == 0 ||
           [passwdText.text length] == 0) {
            [Utility showTipsViewWithText:@"请输入用户名或密码"];
            //显示提示
            return;
        }
        [self requestData:userNameText.text
                   passwd:passwdText.text];
    }
    else if([button isEqual:registerAction]) {
        //注册操作
        if ([userNameText becomeFirstResponder]) {
            [userNameText resignFirstResponder];
        }
        else if([passwdText becomeFirstResponder]) {
            [passwdText resignFirstResponder];
        }
        
        BDRegisterCtr * registerCtr = [[BDRegisterCtr alloc] init];
        [self.navigationController pushViewController:registerCtr
                                             animated:YES];
    }
    else{
        NSLog(@"未知操作");
    }
    
    
}
- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}
- (void)dealloc {
    
}
#pragma mark ---UITextFieldDelegate 

- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    if ([userNameText isEqual:textField]) {
        [textField resignFirstResponder];
        [passwdText becomeFirstResponder];
        return NO;
    }
    else {
        //判断一下两个是否都有值
        if([userNameText.text length] == 0 ||
           [passwdText.text length] == 0) {
            [Utility showTipsViewWithText:@"请输入用户名或密码"];
            //显示提示
            return NO;
        }
        //进行联网请求
        [self requestData:userNameText.text passwd:passwdText.text];
        [passwdText resignFirstResponder];
    }
    return YES;
    
}// called when 'return' key pressed. return NO to ignore.

- (void)requestData:(NSString *)userName passwd:(NSString *)passwd
{
    loginrequest = [[BDRequestData alloc] init];
    NSDictionary * dic = [NSDictionary dictionaryWithObjectsAndKeys:
                                     @"Login",@"Action",
                                     userName,@"UserName",
                                     passwd,@"key",
                                     nil];
    __weak LoginCtr * Weak_self = self;
    [loginrequest loginDataAsync:@""
                             dic:dic onCompletion:^(NSObject *data, NSString *str) {
                                 NSLog(@"str ==%@",str);
                                 NSArray * arr = [str componentsSeparatedByString:@"##"];
                                 if ([arr count] == 2) {
                                     NSString * subStr = [arr objectAtIndex:0];
                                     if ([[subStr lowercaseString] isEqualToString:@"succes"]) {
                                         //表示成功
                                        //发生页面跳转
                                         [[NSUserDefaults standardUserDefaults] setObject:[NSNumber numberWithBool:YES] forKey:LOGIN_KEY];
                                         [[NSUserDefaults standardUserDefaults] setObject:userName forKey:USerNameTxt];
                                         [[NSUserDefaults standardUserDefaults] synchronize];
                                         BDPersonCtr * personCtr = [[BDPersonCtr alloc] init];
                                         
                                         [Weak_self.navigationController pushViewController:personCtr
                                                       animated:YES];
                                     }
                                     else {
                                         //失败
                                         NSString * subStr2 = [arr objectAtIndex:1];
                                         [Utility showTipsViewWithText:subStr2];
                                     }
                                 }
                                 else {
                                     [Utility showTipsViewWithText:@"未知错误"];
                                 }

                             } onError:^(MKNetworkOperation *completedOperation, NSError *error) {
                                 NSString * errorStr = [error localizedDescription];
                                 [Utility showTipsViewWithText:errorStr];
                             }];
    
}


/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end
