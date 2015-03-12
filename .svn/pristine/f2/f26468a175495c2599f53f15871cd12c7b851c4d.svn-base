//
//  carTableViewCell.m
//  CMdriverPro
//
//  Created by zhuwei on 14-9-17.
//  Copyright (c) 2014年 zhuwei. All rights reserved.
//

#import "carTableViewCell.h"
#import "BDRequestData.h"
#import "UIImageView+WebCache.h"
#import "BDLocalData.h"
#import "BDUserDB.h"
#import "NSString+picNamePath.h"
#import "NSStringEX.h"

@implementation carTableViewCell

- (id)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    self = [super initWithStyle:style reuseIdentifier:reuseIdentifier];
    if (self) {
        
        // Initialization code
        UIImage * img  = [UIImage imageNamed:@"main_nocar"];
        NSInteger yTop = (81- img.size.height)/2.0;
        self.icon = [[UIImageView alloc] initWithFrame:CGRectMake(10, yTop, img.size.width , img.size.height)];
        [self.icon setContentMode:UIViewContentModeScaleAspectFill];
        self.icon.clipsToBounds = YES;
        [self.icon setBackgroundColor:[UIColor clearColor]];
        [self.contentView addSubview:self.icon];
        
        self.carPai = [[UILabel alloc] initWithFrame:CGRectMake(CGRectGetMaxX(self.icon.frame) +14, yTop +0, 120, 20)];
        self.carPai.backgroundColor = [UIColor clearColor];
        [self.carPai setFont:[UIFont boldSystemFontOfSize:16]];
        [self.carPai setTextColor:[UIColor colorWithHexValue:0x333333]];
        [self.contentView addSubview:self.carPai];
        
        self.carInTypeImg = [[UIImageView alloc] initWithFrame:CGRectMake(CGRectGetMaxX(self.carPai.frame), self.carPai.frame.origin.y +3, 34, 11.5)];
        [self.contentView addSubview:self.carInTypeImg];
        
        self.localPlace = [[UILabel alloc] initWithFrame:CGRectMake(self.carPai.frame.origin.x, CGRectGetMaxY(self.carPai.frame)+ 3, 200, 20)];
        [self.localPlace setBackgroundColor:[UIColor clearColor]];
        [self.localPlace setFont:[UIFont systemFontOfSize:15]];
        [self.localPlace setTextColor:[UIColor colorWithHexValue:0x666666]];
        [self.contentView addSubview:self.localPlace];
        
        img = [UIImage imageNamed:@"main_btn_find"];
        yTop = (81- img.size.height)/2.0;

        self.findingButton = [UIButton buttonWithType:UIButtonTypeCustom];
        [self.findingButton setFrame:CGRectMake(self.frame.size.width - img.size.width -12, yTop, img.size.width, img.size.height)];
        [self.findingButton setBackgroundImage: img
                                      forState:UIControlStateNormal];
        [self.findingButton addTarget:self
                               action:@selector(findingButton:)
                     forControlEvents:UIControlEventTouchUpInside];
        [self.contentView addSubview:self.findingButton];
        
        UIView *lineView = [[UIView alloc] initWithFrame:CGRectMake(0, 80.5, 320, 0.5)];
        [lineView setBackgroundColor:[UIColor grayColor]];
        [self.contentView addSubview:lineView];

        
    }
    return self;
}

- (void)findingButton:(UIButton *)button
{
    if (self.adelegate && [self.adelegate respondsToSelector:@selector(findMonkey:)]) {
        [self.adelegate findMonkey:self.carCellInfol.carPai];
    }
    
}

- (void)awakeFromNib
{
    // Initialization code
}

- (void)configureData:(BDCarInfo *)carInfoData
{
    self.carCellInfol = carInfoData;
    self.carPai.text = carInfoData.carPai;
    NSString * shengname = @"";
    NSString * cityName = @"";
    BDLocalData * localData = [BDLocalData initWithDataPlist:@"BDLocal"];
    for (BDShengData * sheng in localData.shengArr) {
        if ([sheng.shengCode isEqualToString:carInfoData.sheng]) {
            shengname = sheng.shengName;
            for (BDCityData * cityData in sheng.cityArr) {
                
                if ([cityData.cityCode isEqualToString:carInfoData.city]) {
                    cityName = cityData.cityName;
                    break;
                }
            }
            if ([cityName isEqualToString:@""]) {
                BDCityData * cityData = [sheng.cityArr objectAtIndex:0];
                cityName = cityData.cityName;
            }
            break;
        }
    }
    NSString * tmp = [NSString stringWithFormat:@"调用地:%@-%@",shengname,cityName];
    self.localPlace.text = tmp;
    {
        BDRequestData * request = [[BDRequestData alloc] init];
        NSString * username = [[NSUserDefaults standardUserDefaults] objectForKey:USerNameTxt];
        NSString * passwdStr = [[NSUserDefaults standardUserDefaults] objectForKey:UserPasswdTxt];
        
        NSDictionary * dic = [NSDictionary dictionaryWithObjectsAndKeys:
                              @"getCarPic",@"Action",
                              username,@"UserName",
                              passwdStr,@"Key",
                              carInfoData.carPai,@"CarNum",
                              nil];
        
        __weak carTableViewCell * Weak_self = self;
        NSMutableArray * picFlag = [[NSMutableArray alloc] initWithArray:[carInfoData.picListFlag componentsSeparatedByString:@","]];
        if ([carInfoData.picList count] > 0) {

            NSString * picName = [carInfoData.picList objectAtIndex:0];
            NSString * strPath = [picName picNamePath];

            [Weak_self.icon setImage:[UIImage imageWithContentsOfFile:strPath]];

        }
        else {
            [request loginDataAsync:@""
                                dic:dic
                       onCompletion:^(NSObject *data, NSString *str) {
                           
                           NSLog(@"str ==%@",str);
                           NSArray * arr = [str componentsSeparatedByString:@"##"];
                           if ([arr count] == 2) {
                               NSString * subStr = [arr objectAtIndex:0];
                               if ([[subStr lowercaseString] isEqualToString:@"success"]) {
                                   //表示成功
                                   //发生页面跳转
                                   
                                   NSString * secondStr = [arr objectAtIndex:1];
                                   if ([[secondStr lowercaseString] isEqualToString:@"nopic"]) {
                                       if ([carInfoData.picList count] > 0) {

                                           NSString * picName = [carInfoData.picList objectAtIndex:0];
                                           NSString * strPath = [picName picNamePath];

                                           [Weak_self.icon setImage:[UIImage imageWithContentsOfFile:strPath]];
                                           
                                       }
                                       else {
                                           [Weak_self.icon setImage:[UIImage imageNamed:@"main_nocar"]];
                                           
                                       }
                                       
                                   }
                                   else {
                                       carInfoData.picList = [[NSMutableArray alloc] init];
                                       [Weak_self.icon setImageWithURL:[NSURL URLWithString:secondStr]
                                                      placeholderImage:[UIImage imageNamed:@"main_nocar"] options:SDWebImageLowPriority success:^(UIImage *image, BOOL cached) {
                                                          
                                                          NSData *imageData = UIImageJPEGRepresentation(image, 0);
                                                          

                                                          NSString * picName = [BDCarInfo getPicName:carInfoData.carPai];
                                                          NSString * strPath = [picName picNamePath];

                                                          
                                                          [imageData writeToFile:strPath
                                                                      atomically:YES];
                                                          [carInfoData.picList  addObject:picName];
                                                          
                                                          BDUserDB * userDb = [BDUserDB shareInstance];
                                                          [picFlag replaceObjectAtIndex:[carInfoData.picList indexOfObject:picName]
                                                                             withObject:@"1"];
                                                          [userDb updatePic:carInfoData.carPai
                                                                        pic:carInfoData.picList
                                                                   flagList:picFlag];
                                                          
                                                          
                                                      } failure:^(NSError *error) {
                                                          
                                                      }];
                                   }
                                   
                               }
                               else {
                                   [Weak_self.icon setImage:[UIImage imageNamed:@"main_nocar"]];
                               }
                           }
                           
                           
                       }
                    onError:^(MKNetworkOperation *completedOperation, NSError *error) {
                        [Weak_self.icon setImage:[UIImage imageNamed:@"main_nocar"]];
                    }];

        }
        if (carInfoData.type == car_empty) {
            [self.carInTypeImg setImage:[UIImage imageNamed:@"main_carstatus0"]];
            
        }
        else if(carInfoData.type == car_finding){
            [self.carInTypeImg setImage:[UIImage imageNamed:@"main_carstatus1"]];
        }
        else if(carInfoData.type == car_sending) {
            [self.carInTypeImg setImage:[UIImage imageNamed:@"main_carstatus2"]];

        }

    }
}

- (void)setSelected:(BOOL)selected animated:(BOOL)animated
{
    [super setSelected:selected animated:animated];

    // Configure the view for the selected state
}

@end
