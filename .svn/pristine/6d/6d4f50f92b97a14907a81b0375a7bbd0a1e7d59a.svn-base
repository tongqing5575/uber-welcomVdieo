//
//  RootViewController.m
//  AVPlayer
//
//  Created by www.xywy.com on 15/5/15.
//  Copyright (c) 2015年 www.xywy.com. All rights reserved.
//

#import "RootViewController.h"
#import "PulsingHaloLayer.h"

#import <AVFoundation/AVFoundation.h>
#import <CoreMedia/CoreMedia.h>

@interface RootViewController ()

@property (nonatomic, strong) PulsingHaloLayer *halo; // 脉冲
@property (nonatomic, weak) UIImageView *beaconView; // 图片动画
@property (nonatomic,weak) AVPlayer *Playerview; // 播放器

@end

@implementation RootViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    
    [self playerAVPlayer]; // 播放器
    
    [self maiChongXiaoGuo]; // 脉冲效果
}

/** 播放器 */
- (void)playerAVPlayer
{
    // 播放mp4
    NSString *filePath = [[NSBundle mainBundle] pathForResource:@"welcome_video" ofType:@"mp4"];
    NSURL *sourceMovieURL = [NSURL fileURLWithPath:filePath];
    AVAsset *movieAsset = [AVURLAsset URLAssetWithURL:sourceMovieURL options:nil];
    
    AVPlayerItem * playerItem = [AVPlayerItem playerItemWithAsset:movieAsset];
    self.Playerview = [AVPlayer playerWithPlayerItem:playerItem];
    AVPlayerLayer *playerLayer = [AVPlayerLayer playerLayerWithPlayer:self.Playerview];
    
    playerLayer.frame = CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height);
    playerLayer.videoGravity = AVLayerVideoGravityResizeAspect;
    self.Playerview.actionAtItemEnd = AVPlayerItemStatusFailed;
    
    [self.view.layer addSublayer:playerLayer];
    
    [self.Playerview play];
    
    //注册通知
    [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(runLoopTheMovie:) name:AVPlayerItemDidPlayToEndTimeNotification object:nil];
}

/** 脉冲效果 */
- (void)maiChongXiaoGuo
{
    
#warning mark-原来halo.position是beaconView的center, 但是会出问题, 我暂时改halo的位置设置为self.view的中心点以及改了添加方法
    // 脉冲效果
    self.halo = [PulsingHaloLayer layer];
    self.halo.position= self.view.center;
    [self.view.layer addSublayer:self.halo];
    
    // 六秒后删除PulsingHaloLayer
    [NSTimer scheduledTimerWithTimeInterval:6 target:self selector:@selector(addRemovePlayer) userInfo:nil repeats:NO];
    
    // 延时六秒执行方法
    [NSTimer scheduledTimerWithTimeInterval:5 target:self selector:@selector(addImageView) userInfo:nil repeats:YES];
}

/** 创建图片动画 */
- (void)tomAnimation:(NSString *)img count:(int)count
{
    // 图片动画
    UIImageView *beaconView = [[UIImageView alloc] init];
    beaconView.frame = CGRectMake(40, 160, 300, 300);
    self.beaconView = beaconView;
    [self.view addSubview:beaconView];
    
    // 如果正在动画，直接返回
    if ([self.beaconView isAnimating]) return;
    
    // 图片的数组
    NSMutableArray *arrayM = [NSMutableArray array];
    for (int i = 0; i < count; i++) {
        NSString *imageName = [NSString stringWithFormat:@"%@-%03d@2x.png", img, i];
        
        NSString *path = [[NSBundle mainBundle] pathForResource:imageName ofType:nil];
        UIImage *image = [UIImage imageWithContentsOfFile:path];
        
        [arrayM addObject:image];
    }
    
    [self.beaconView setAnimationImages:arrayM];
    
    // 设置动画时长
    [self.beaconView setAnimationDuration:arrayM.count * 0.075];
    [self.beaconView setAnimationRepeatCount:1];
    
    // 开始动画
    [self.beaconView startAnimating];
    
    // 播放完成后清空数组
    [self performSelector:@selector(clearTom) withObject:nil afterDelay:self.beaconView.animationDuration];
}

/** 播放图片动画 */
- (void)addImageView
{
    [self tomAnimation:@"logo" count:68];
}

/** 删除player */
- (void)addRemovePlayer
{
    [self.halo removeFromSuperlayer];
}

/** 重复播放 */
- (void)runLoopTheMovie:(NSNotification *)notification
{
    //注册的通知  可以自动把 AVPlayerItem 对象传过来，只要接收一下就OK
    AVPlayerItem *player = [notification object];
    //关键代码
    [player seekToTime:kCMTimeZero];
    [self.Playerview play];
}

/** 清空图片动画并显示最后一张 */
- (void)clearTom
{
        self.beaconView.animationImages = nil;
        self.beaconView.image = [UIImage imageNamed:@"logo-067@2x.png"];
}
@end
