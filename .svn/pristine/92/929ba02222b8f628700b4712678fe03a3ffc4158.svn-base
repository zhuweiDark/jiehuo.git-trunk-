//
//  BDRequestData.h
//  CMdriverPro
//
//  Created by zhuwei on 14-9-12.
//  Copyright (c) 2014年 zhuwei. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "MKNetworkOperation.h"

typedef void(^BDRequestCompletionResponseBlock)(NSObject * data,NSString * str);
typedef void(^BDRequestCompletionResponseErrorBlock)(MKNetworkOperation*completedOperation, NSError* error);
@interface BDRequestData : NSObject

-(void)loginDataAsync:(NSString *)path
                  dic:(NSDictionary *)dic
         onCompletion:(BDRequestCompletionResponseBlock) completionBlock
              onError:(BDRequestCompletionResponseErrorBlock) errorBlock;

@end
