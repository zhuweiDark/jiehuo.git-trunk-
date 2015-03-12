//
//  BDCarTableView.m
//  CMdriverPro
//
//  Created by zhuwei on 14-9-17.
//  Copyright (c) 2014年 zhuwei. All rights reserved.
//

#import "BDCarTableView.h"
#import "carTableViewCell.h"

@interface BDCarTableView()<carTableViewCellDelegate>

@end
@implementation BDCarTableView
@synthesize listCar = _listCar;

- (void)setListCar:(NSMutableArray *)listCar
{
    _listCar = listCar;
    [self.carTableView reloadData];
}

- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        // Initialization code
        self.backgroundColor = [UIColor whiteColor];
        [self createTableView];
    }
    return self;
}

- (void)createTableView
{
    self.carTableView = [[UITableView alloc] initWithFrame:self.bounds
                                                     style:UITableViewStylePlain];
    [self.carTableView setBackgroundColor:[UIColor clearColor]];
    [self.carTableView setDelegate:self];
    [self.carTableView setDataSource:self];
    [self.carTableView setRowHeight:81];
    [self.carTableView setSeparatorStyle:UITableViewCellSeparatorStyleNone];
    [self addSubview:self.carTableView];
    
}

#pragma mark --UITableViewDataSource
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return [self.listCar count];
}

// Row display. Implementers should *always* try to reuse cells by setting each cell's reuseIdentifier and querying for available reusable cells with dequeueReusableCellWithIdentifier:
// Cell gets various attributes set automatically based on table (separators) and data source (accessory views, editing controls)

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString * cellStr = @"cellStr";
    carTableViewCell * cell = [tableView dequeueReusableCellWithIdentifier:@"cellStr"];
    if (!cell) {
        cell = [[carTableViewCell alloc] initWithStyle:UITableViewCellStyleDefault
                                       reuseIdentifier:cellStr];
        
    }
    cell.adelegate =self;
    NSInteger row = indexPath.row;
    [cell configureData:[self.listCar objectAtIndex:row]];
     
    return cell;
}

#pragma mark -- UITableViewDelegate
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    //编辑车辆
    NSInteger row = indexPath.row;
    if (self.aadelegate && [self.aadelegate respondsToSelector:@selector(createAddCarView:)]) {
        [self.aadelegate createAddCarView:[self.listCar objectAtIndex:row]];
    }
    [tableView deselectRowAtIndexPath:indexPath
                             animated:NO];
    
}


#pragma mark -- carTableViewCellDelegate

- (void) findMonkey:(NSString *)carPai
{
    //显示找货界面
    if (self.aadelegate && [self.aadelegate respondsToSelector:@selector(findHuo:)]) {
        [self.aadelegate findHuo:carPai];
    }
}

/*
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect
{
    // Drawing code
}
*/

@end
