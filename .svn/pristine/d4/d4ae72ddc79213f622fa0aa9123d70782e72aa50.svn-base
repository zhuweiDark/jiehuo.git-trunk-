//
//  BDRequestData.m
//  CMdriverPro
//
//  Created by zhuwei on 14-9-12.
//  Copyright (c) 2014年 zhuwei. All rights reserved.
//

#import "BDRequestData.h"
#import "VEngine.h"

@interface BDRequestData()
{
    VEngine  * _engine;
    
}
@property (nonatomic,strong) MKNetworkOperation *netoperator;

@end
@implementation BDRequestData
-(id) init
{
    self  = [super init];
    if (self) {
        _engine = [[VEngine alloc] initWithHostName:@"www.sijiykt.com/AndroidDriver.shtml"];
    }
    
    return self;
}
-(void)loginDataAsync:(NSString *)path
                  dic:(NSDictionary *)dic
         onCompletion:(BDRequestCompletionResponseBlock) completionBlock
              onError:(BDRequestCompletionResponseErrorBlock) errorBlock
{
    self.netoperator = [_engine operationWithPath:path
                                               params:dic
                                           httpMethod:@"GET"
                                  requireVersionParam:YES];
    
    
    __weak BDRequestData * c_self = self;
    [self.netoperator addCompletionHandler:^(MKNetworkOperation *completedOperation)
     {
         //获取completedOperation的返回数据
         
         //gbk编码
         unsigned long encode = CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000);
         NSData *responseData = [completedOperation responseData];
         NSString *valueString = [[NSString alloc] initWithData:responseData encoding:encode];
        
         completionBlock(completedOperation.responseData,valueString);
         [c_self cleanNetOperation];
         
         
     }errorHandler:^(MKNetworkOperation* completedOperation, NSError* error) {
         //错误处理
         errorBlock(completedOperation, error);
         [c_self cleanNetOperation];
     }];
    //加入处理函数
    [_engine enqueueOperation:self.netoperator forceReload:YES];
    

}
- (void)cleanNetOperation
{
    if (self.netoperator) {
        [self.netoperator cancel];
        self.netoperator = nil;
    }
}
@end
