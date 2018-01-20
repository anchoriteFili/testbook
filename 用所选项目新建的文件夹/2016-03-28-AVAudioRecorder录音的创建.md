---
layout: post
#标题
title: AVAudioRecorder录音的创建
#时间配置
date:   2016-03-28 21:26:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/AVAudioRecorder录音.zip" target="_blank">AVAudioRecorder录音.zip</a><br>

```objc
#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>

@interface ViewController ()<AVAudioRecorderDelegate> {
    NSTimer *timer; //定时器
    
}

@property (nonatomic,retain) AVPlayer *player; //音频播放器
@property (nonatomic,retain) NSURL *audioUrl; //录音的文件地址
@property (nonatomic,retain) AVAudioRecorder *recorder; //创建录音对象

@property (weak, nonatomic) IBOutlet UIButton *audioButton; //录音按钮

@property (weak, nonatomic) IBOutlet UILabel *timerLabel; //用于显示时间


@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    [self setAudio]; //先初始化录音器
}

#pragma mark 按下时点击事件
- (IBAction)audioClickDown:(UIButton *)sender {
    
    [sender setTitle:@"正在录音" forState:UIControlStateNormal];
    
    //创建录音文件，准备录音
    if ([self.recorder prepareToRecord]) {
        //开始
        [self.recorder record];
    }
    
    //设置定时检测
    timer = [NSTimer scheduledTimerWithTimeInterval:0 target:self selector:@selector(detectionVoice) userInfo:nil repeats:YES];
}

#pragma mark 刷新音频录制的时间
- (void)detectionVoice {
    
    self.timerLabel.text = [NSString stringWithFormat:@"录音时长 %f",self.recorder.currentTime];
}

#pragma mark 抬起时点击事件
- (IBAction)audioClickUp:(UIButton *)sender {
    
    [sender setTitle:@"开始录音" forState:UIControlStateNormal];
    
    double cTime = self.recorder.currentTime;
    if (cTime > 2) {//如果录制时间<2 不发送
        NSLog(@"发出去");
    }else {
        //删除记录的文件
        [self.recorder deleteRecording];
        //删除存储的
    }
    [self.recorder stop];
    [timer invalidate];
}

#pragma mark 播放录音
- (IBAction)palyAudioBtnClick:(UIButton *)sender {
    
#pragma mark 创建播放器
    AVPlayerItem *item = [[AVPlayerItem alloc] initWithURL:self.audioUrl];
    
    self.player = [[AVPlayer alloc] initWithPlayerItem:item];
    [self.player play];
}


#pragma mark 配置录音的各种信息
- (void)setAudio {
    
    //进行录音设置
    NSMutableDictionary *recordSetting = [[NSMutableDictionary alloc] init];
    
    //设置录音格式
    [recordSetting setValue:[NSNumber numberWithInt:kAudioFormatMPEG4AAC] forKey:AVFormatIDKey];
    
    //设置录音采样率(HZ) 如：AVSampleRateKey == 8000/44100/96000 （影响音频的质量）
    [recordSetting setValue:[NSNumber numberWithFloat:44100] forKey:AVSampleRateKey];
    
    //录音通道数 1或2
    [recordSetting setValue:[NSNumber numberWithInt:1] forKey:AVNumberOfChannelsKey];
    
    //线性采样位数 8、16、24、32
    [recordSetting setValue:[NSNumber numberWithInt:16] forKey:AVLinearPCMBitDepthKey];
    
    //录音的质量
    [recordSetting setValue:[NSNumber numberWithInt:AVAudioQualityHigh] forKey:AVEncoderAudioQualityKey];
    
    NSString *strUrl = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
    
    NSURL *url = [NSURL fileURLWithPath:[NSString stringWithFormat:@"%@/lll.aac",strUrl]];
    
    self.audioUrl = url;
    
    //初始化录音器
    NSError *error;
    self.recorder = [[AVAudioRecorder alloc] initWithURL:url settings:recordSetting error:&error];
    //开始录音检测
    self.recorder.meteringEnabled = YES;
    self.recorder.delegate = self;
    
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```