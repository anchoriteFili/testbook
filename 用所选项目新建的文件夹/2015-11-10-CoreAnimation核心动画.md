---
layout: post
#标题
title:  CoreAnimation核心动画
#时间配置
date:   2015-11-10 16:45:12 +0800
#大类配置
categories: 知识
#小类配置
tag: object-c
---

* content
{:toc}

#### 1. 简单动画
---

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/CoreAnimation简单动画.zip" target="_blank">CoreAnimation简单动画.zip</a><br>
<a href="http://www.jianshu.com/p/02c341c748f9" target="_blank">CABasicAnimation使用总结</a><br>





**动画创建一般分为以下几步：**

* 1.初始化动画并设置动画属性
* 2.设置动画属性初始值（可以省略）、结束值以及其他动画属性
* 3.给图层添加动画

```objc
#import "MoveAnimationShowVC.h"

@interface MoveAnimationShowVC () {
    CALayer *_layer;
}

@end

/**
 1. 初始化动画并设置动画属性
 2. 设置动画属性初始值（可以省略）、结束值以及其他动画属性
 3. 给图层添加动画
 */
@implementation MoveAnimationShowVC

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    //设置背景（注意这个图片其实在根图层）
    UIImage *backgorundImage = [UIImage imageNamed:@"底板"];
    self.view.backgroundColor = [UIColor colorWithPatternImage:backgorundImage];
    
    //自定义一个图层
    _layer = [[CALayer alloc] init];
    _layer.bounds = CGRectMake(0, 0, 10, 20);
    _layer.position = CGPointMake(50, 150);
    _layer.contents = (id)[UIImage imageNamed:@"花瓣"].CGImage;
    [self.view.layer addSublayer:_layer];
    
}

#pragma mark 移动动画
- (void)translationAnimation:(CGPoint)location {
    
    //1. 创建动画并指定动画属性
    CABasicAnimation *basicAnimation = [CABasicAnimation animationWithKeyPath:@"position"];
    
    //2. 设置动画属性初始值和结束值
//    basicAnimation.fromValue = [NSNumber numberWithInteger:50]; //可以不设置，默认为图层初始状态
    basicAnimation.toValue = [NSValue valueWithCGPoint:location];
    
    //设置其他动画属性
    basicAnimation.duration = 5.0; //动画时间5秒
//    basicAnimation.repeatCount = HUGE_VALF; //设置重复次数，HUGE_VALF可看做无穷大，起到循环动画的效果
//    basicAnimation.removedOnCompletion = NO; //运行一次是否移除动画
    
    //存储当前位置在动画结束后使用
    [basicAnimation setValue:[NSValue valueWithCGPoint:location] forKey:@"KCBasicAnimationLocation"];
    
    basicAnimation.delegate = self;
    
    //3. 添加动画到图层，注意key相当于给动画进行命名，以后获得该动画时可以使用此名称获取
    [_layer addAnimation:basicAnimation forKey:@"KCBasicAnimation_Translation"];
    
}

#pragma mark 点击事件
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    
    UITouch *touch = touches.anyObject;
    CGPoint location = [touch locationInView:self.view];
    
    //创建并开始动画
    [self translationAnimation:location];
}

#pragma mark ****************动画代理方法****************
#pragma mark 动画开始
- (void)animationDidStart:(CAAnimation *)anim {
    
    NSLog(@"animation(%@) start.\r_layer.frame=%@",anim,NSStringFromCGRect(_layer.frame));
    NSLog(@"%@",[_layer animationForKey:@"KCBasicAnimation_Translation"]); //通过前面的设置的key获得动画
}

#pragma mark 动画结束
- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag {
    
    NSLog(@"animation(%@) start.\r_layer.frame=%@",anim,NSStringFromCGRect(_layer.frame));
   
    //开启事务
    [CATransaction begin];
    //禁用隐式动画
    [CATransaction setDisableActions:YES];

    _layer.position = [[anim valueForKey:@"KCBasicAnimationLocation"] CGPointValue];
    
    //提交事务
    [CATransaction commit];
    
}
```

#### 2. 关键帧动画
---

<a href="" target="_blank">CoreAnimation关键帧动画.zip</a><br>


> 在关键帧动画中还有一些动画属性出血症往往比较容易混淆，这里专门针对这些属性做一下介绍
> 
> keyTimes: 各个关键帧的时间控制。前面使用values设置了四个关键帧，默认情况下每两个帧之间的间隔为:8/(4-1)秒
> 
> 如果要控制动画从第一帧到第二帧占用时间4秒，从第二帧到第三帧时间为2秒，而从第三帧到第四帧时间2秒的话，就可以通过
> 
> keyTimes进行设置。keyTimes中存储的是时间占用比例点，此时可以设置keyTimes的值为0.0，0.5，0.75，1.0（当然
> 
> 必须转换成NSNumber），也就是说1到2帧运行到总时间的50%，2到3帧运行到总时间的75%，3到4帧运行到8秒结束。
> 
> caculationMode: 动画计算模式。还拿上面keyValues动画距离，之所以1到2帧能形成连贯性动画而不是直接从第一帧
> 
> 经过8/3秒到第2帧是因为动画模式是连续的（值为kCAAnimationLinear，这是计算模式的的默认值）；而如果制定了动
> 
> 画模式为kCAAnimationDiscrete离散的那么你会看到动画从第一帧进过8/3秒直接到第2帧，中间没有任何过度。其他动
> 
> 画模式为kCAAnimationPaced（均匀执行，会忽略keyTimes)、kCAAnimationCubic(平滑执行，对于位置变动关键帧
> 
> 动画运行轨迹跟平滑）、kCAAnimationCubicPaced（平滑均匀执行）。

```objc
#import "ViewController.h"

@interface ViewController () {
    CALayer *_layer;
    int type;
}

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    /**
     关键帧动画就是在动画控制过程中开发者指定主要的动画状态，至于两个状态间动画如何进行则又系统自动补充
     (每两个关键帧之间系统形成的动画成为“补间动画”)，这种动画的好处就是开发者不用诸葛控制每个动画帧，而
     只要关心几个关键帧的状态即可。
     关键帧动画开发分为两种形式：一种是通过设置不同的属性值进行关键帧控制，另一种是通过绘制路径进行关键帧
     控制。后者优先级高于前者，如果设置了路径则属性值就不再起作用。
     */
    
    //设置背景
    UIImage *backgroundImage = [UIImage imageNamed:@"底板"];
    self.view.backgroundColor = [UIColor colorWithPatternImage:backgroundImage];
    
    //自定义一个图层
    _layer = [[CALayer alloc] init];
    _layer.bounds = CGRectMake(0, 0, 10, 20);
    _layer.position = CGPointMake(50, 150);
    _layer.contents = (id)[UIImage imageNamed:@"花瓣"].CGImage;
    [self.view.layer addSublayer:_layer];
    
    [self translationAnimation]; //启动关键帧动画
    
}

#pragma mark 关键帧动画
- (void)translationAnimation {
    
    /**
     type:
     1: 关键帧动画的属性动画
     2: 关键帧动画的路径动画（利用贝塞尔曲线）
     */
    type = 2;
    
    
    if (type == 1) {
#pragma mark 属性动画
        //1. 创建关键帧动画并设置动画属性
        CAKeyframeAnimation *keyframeAnimation = [CAKeyframeAnimation animationWithKeyPath:@"position"];
        
        //2. 设置关键帧，这里有四个关键帧
        NSValue *key1 = [NSValue valueWithCGPoint:_layer.position]; //对于关键正动画初始值不能省略
        NSValue *key2 = [NSValue valueWithCGPoint:CGPointMake(80, 220)];
        NSValue *key3 = [NSValue valueWithCGPoint:CGPointMake(45, 300)];
        NSValue *key4 = [NSValue valueWithCGPoint:CGPointMake(55, 400)];
        NSArray *values = @[key1,key2,key3,key4];
        keyframeAnimation.values = values;
        
        keyframeAnimation.delegate = self;
        
        //设置其他属性
        keyframeAnimation.duration = 8.0;
        keyframeAnimation.beginTime = CACurrentMediaTime()+2; //设置延迟两秒执行
        
        //3. 添加动画到图层，添加动画后就会执行动画
        [_layer addAnimation:keyframeAnimation forKey:@"KCKeyframeAnimation_Position"];
    } else if (type == 2) {
#pragma mark 创建路径动画
        //1. 创建关键帧动画并设置动画属性
        CAKeyframeAnimation *keyframeAnimation = [CAKeyframeAnimation animationWithKeyPath:@"position"];
        
        //2. 设置路径
        //绘制贝塞尔曲线
        CGMutablePathRef path = CGPathCreateMutable();
        CGPathMoveToPoint(path, NULL, _layer.position.x, _layer.position.y); //移动到起始点
        CGPathAddCurveToPoint(path, NULL, 160, 280, -30, 300, 55, 400); //绘制二次贝塞尔曲线
        
        keyframeAnimation.path = path; //设置path属性
        CGPathRelease(path); //释放路径对象
        
        keyframeAnimation.delegate = self;
        // 设置其他属性
        keyframeAnimation.duration = 5.0;
        keyframeAnimation.beginTime = CACurrentMediaTime()+2; //设置延迟5秒执行
        
        //3. 添加动画到图层，添加动画后就会执行动画
        [_layer addAnimation:keyframeAnimation forKey:@"KCKeyframeAnimation_Position"];
    }
    
}

#pragma mark 动画代理方法
- (void)animationDidStart:(CAAnimation *)anim {
    //动画开始代理方法
}

#pragma mark 动画结束方法
- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag {
    
    /**
     CATransaction事务
     分为隐式和显式
     [隐式]在某次RunLoop中设置了一个“Animatable”属性，如果当前没有设置事务，则灰自动创建一个CATranscation,
     并在当前线程的下一个RunLoop中commit这个CATransaction
     [显式]就是直接调用CATransaction的[CATransaction begin],[CATransaction commit]等相关方法。比如线面的不希望self.subLayer.postion产生动画，这直接设置
     */
    //开启事务
    [CATransaction begin];
    [CATransaction setDisableActions:YES]; //关闭动画
    _layer.position = [[anim valueForKey:@"KCKeyframeAnimation_Position"] CGPointValue];
    
    //提交事务
    [CATransaction commit];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```

#### 3. 动画组
---

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/CoreAnimation动画组.zip" target="_blank">CoreAnimation动画组.zip</a><br>

```objc
#import "ViewController.h"

@interface ViewController () {
    CALayer *_layer;
    int changeValue;
}

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    /**
     动画组使用起来并不复杂，首先单独创建单个动画（可以是基础动画也可以是关键帧动画），然后将基础动画添加到动画组，最后将动画组添加到图层即可
     */
    changeValue = 50;
    
    //设置背景
    UIImage *backgroundImage = [UIImage imageNamed:@"底板"];
    self.view.backgroundColor = [UIColor colorWithPatternImage:backgroundImage];
    
#pragma mark 定时器执行方法
    [NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(createGroupAnimaitons) userInfo:nil repeats:YES];
    
}

- (void)createGroupAnimaitons {
    
    //自定义一个图层
    _layer = [[CALayer alloc] init];
    _layer.bounds = CGRectMake(0, 0, 10, 20);
    _layer.position = CGPointMake(changeValue, 150);
    _layer.contents = (id)[UIImage imageNamed:@"花瓣"].CGImage;
    [self.view.layer addSublayer:_layer];
    
    //创建动画
    [self groupAnimation];
    
    changeValue += 10;
    if (changeValue >= self.view.frame.size.width) {
        changeValue = 50;
    }
}


#pragma mark 基础旋转动画
- (CABasicAnimation *)rotationAnimation {
    
    CABasicAnimation *basicAnimation = [CABasicAnimation animationWithKeyPath:@"transform.rotation.z"];
    
    CGFloat toValue = M_PI_2*3;
    basicAnimation.toValue = [NSNumber numberWithFloat:toValue];
    
//    basicAnimation.duration = 6.0;
    basicAnimation.autoreverses = true; //自动调整
    basicAnimation.repeatCount = HUGE_VALF;
    basicAnimation.removedOnCompletion = NO;
    
    [basicAnimation setValue:[NSNumber numberWithFloat:toValue] forKey:@"KCBasicAnimationProperty_ToValue"];
    return basicAnimation;
}

#pragma mark 关键帧移动动画
- (CAKeyframeAnimation *)translationAnimation {
    
    CAKeyframeAnimation *keyframeAnimation = [CAKeyframeAnimation animationWithKeyPath:@"position"];
    
    CGPoint endPoint = CGPointMake(changeValue, 400);
    CGMutablePathRef path = CGPathCreateMutable();
    CGPathMoveToPoint(path, NULL, _layer.position.x, _layer.position.y);
    CGPathAddCurveToPoint(path, NULL, 160, 280, -30, 300, endPoint.x, endPoint.y);
    
    keyframeAnimation.path = path;
    CGPathRelease(path);
    
    [keyframeAnimation setValue:[NSValue valueWithCGPoint:endPoint] forKey:@"KCKeyframeAnimationProperty_EndPosition"];
    
    return keyframeAnimation;
}

#pragma mark 创建动画组
- (void)groupAnimation {
    
    //1. 创建动画组
    CAAnimationGroup *animationGroup = [CAAnimationGroup animation];
    
    //2. 设置组中的动画和其他属性
    CABasicAnimation *basicAnimaiton = [self rotationAnimation];
    CAKeyframeAnimation *keyframeAnimation = [self translationAnimation];
    animationGroup.animations = @[basicAnimaiton,keyframeAnimation];
    
    animationGroup.delegate = self;
    
    animationGroup.duration = 10.0; //社会自动画时间，如果动画组中动画已经设置过动画属性则不再生效
    animationGroup.beginTime = CACurrentMediaTime()+2; //延迟2秒执行
    
    //3. 给图层添加动画
    [_layer addAnimation:animationGroup forKey:nil];
}

#pragma mark 代理方法
#pragma mark 动画完成
- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag {
    
    CAAnimationGroup *animationGroup = (CAAnimationGroup *)anim;
    CABasicAnimation *basicAnimation = (CABasicAnimation *)animationGroup.animations[0];
    CAKeyframeAnimation *keyframeAnimation = (CAKeyframeAnimation *)animationGroup.animations[1];
    CGFloat toValue = [[basicAnimation valueForKey:@"KCBasicAnimationProperty_ToValue"] floatValue];
    CGPoint endPoint = [[keyframeAnimation valueForKey:@"KCKeyframeAnimationProperty_EndPosition"] CGPointValue];
    
    [CATransaction begin];
    [CATransaction setDisableActions:YES];
    
    //设置动画最终状态
    _layer.position = endPoint;
    _layer.transform = CATransform3DMakeRotation(toValue, 0, 0, 1);
    
    [CATransaction commit];
    
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```

#### 4. 逐帧动画
---

<a href="http://files.cnblogs.com/files/AnchoriteFiliGod/CoreAnimation逐帧动画.zip" target="_blank">CoreAnimation逐帧动画.zip</a><br>

```objc
#import "ViewController.h"

#define IMAGE_COUNT 10

@interface ViewController () {
    CALayer *_layer;
    int _index;
    NSMutableArray *_images;
}

@end

/**
 ReadMe
 虽然在核心动画没有直接提供逐帧动画类型，但是却提供了用于完成逐帧动画的相关对象CADisplayLink。
 CADiplayLink是一个计时器，但是同NSTimer不同的是，CADisplayLink的刷新周期同屏幕完全一致。
 例如在iOS中屏幕刷新周期是60次/秒，CADisplayLink刷新周期同屏幕刷新一致也是60次/秒，这样以来
 使用它完全的逐帧动画（又称“时钟动画”）完全感觉不到动画的停滞情况
 */

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    //设置背景
    self.view.layer.contents = (id)[UIImage imageNamed:@"底板"].CGImage;
    
    //创建图像显示图层
    _layer = [[CALayer alloc] init];
    _layer.bounds = CGRectMake(0, 0, 87, 32);
    _layer.position = CGPointMake(100, 340);
    [self.view.layer addSublayer:_layer];
    
    //由于与的图片在循环中会不断创建，而10张鱼的图片相对都很小
    //与其在循环中不断创建UIImage不如直接将10张图片缓存起来
    
    _images = [NSMutableArray array];
    for (int i = 0; i < 3; ++ i) {
        NSString *imageName = [NSString stringWithFormat:@"%d",i+1];
        
        UIImage *image = [UIImage imageNamed:imageName];
        [_images addObject:image];
    }
    
    //定义时钟对象
    CADisplayLink *displayLink = [CADisplayLink displayLinkWithTarget:self selector:@selector(step)];
    //添加时钟对象到主运行循环
    [displayLink addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];
}

#pragma mark 每次屏幕刷新都会执行一次次方法(每秒接近60次）
- (void)step {
    //定义一个变量记录执行次数
    static int s = 0;
    if (_index >= 3) {
        _index = 1;
    }
    //每秒执行6次
    if (++s%3 == 0) {
        UIImage *image = _images[_index];
        _layer.contents = (id)image.CGImage; //更新图片
        _index = (_index+1)%IMAGE_COUNT;
    }
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```

#### 5. 转场动画
---


**转场动画**

> 转场动画就是从一个常见以动画的形式过渡到另一个场景。转场动画的使用一般分为以下几个步骤

* 1> 创建转场动画

* 2> 设置转场类型、子类型（可选）及其他属性

* 3> 设置转场后的新视图并添加动画到图层

 

**常用的转场类型**

**公开API**

名称|说明|调取
-|:-|:-
fade|淡出效果|kCATransitionFade
movein|新视图移动到旧视图上|kCATransitionMoveIn
push|新视图推出旧视图|kCATransitionPush
reveal|移开旧视图显示新视图|kCATransitionReveal

**私有API 私用API只能通过字符串访问**

名称|说明
-|-
cube|立方体翻转效果
oglFlip|翻转效果
suckEffect|收缩效果
rippleEffect|水滴波纹效果
pageCurl|向上翻页效果
pageUnCurl|向下翻页效果
cameralIrisHollowOpen|摄像头打开效果
cameralIrisHollowClose|摄像头关闭效果


```objc
#import "ViewController.h"

#define IMAGE_COUNT 5

@interface ViewController () {
    UIImageView *_imageView;
    int _currentIndex;
}

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    
    //定义图片控件
    _imageView = [[UIImageView alloc] init];
    _imageView.frame = CGRectMake(0, 0, self.view.frame.size.width,self.view.frame.size.height);
    _imageView.contentMode = UIViewContentModeScaleAspectFit;
    _imageView.image =  [UIImage imageNamed:@"0"];
    [self.view addSubview:_imageView];
    
    
    //添加手势
    UISwipeGestureRecognizer *leftSwipeGesture = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(leftSwipe:)];
    leftSwipeGesture.direction = UISwipeGestureRecognizerDirectionLeft;
    [self.view addGestureRecognizer:leftSwipeGesture];
    
    UISwipeGestureRecognizer *rightSwipeGesture = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(rightSwipe:)];
    rightSwipeGesture.direction = UISwipeGestureRecognizerDirectionRight;
    [self.view addGestureRecognizer:rightSwipeGesture];
    
}

#pragma mark 向左划
- (void)leftSwipe:(UISwipeGestureRecognizer *)gesture {
    [self transitionAnimation:YES];
}

#pragma mark 向右划
- (void)rightSwipe:(UISwipeGestureRecognizer *)gesture {
    [self transitionAnimation:NO];
}

#pragma mark 转场动画
- (void)transitionAnimation:(BOOL)isNext {
    
    //1. 创建转场动画对象
    CATransition *transition = [[CATransition alloc] init];
    
    /**
     公开API
     fade                    淡出效果                kCATransitionFade
     movein                  新视图移动到旧视图上      kCATransitionMoveIn
     push                    新视图推出旧视图         kCATransitionPush
     reveal                  移开旧视图显示新视图      kCATransitionReveal
     私有API 私用API只能通过子妇产访问
     cube                    立方体翻转效果
     oglFlip                 翻转效果
     suckEffect              收缩效果
     rippleEffect            水滴波纹效果
     pageCurl                向上翻页效果
     pageUnCurl              向下翻页效果
     cameralIrisHollowOpen   摄像头打开效果
     cameralIrisHollowClose  摄像头关闭效果
     */
    
    //2. 设置动画类型，注意对于苹果官方没公开的动画类型只能使用子妇产，并没有对应的常量定义
    transition.type = @"push";
    
    //设置子类型
    if (isNext) {
        transition.subtype = kCATransitionFromRight;
    } else {
        transition.subtype = kCATransitionFromLeft;
    }
    
    //设置动画时长
    transition.duration = 1.0f;
    
    //3. 设置转场后的新视图添加转场动画
    _imageView.image = [self getImage:isNext];
    [_imageView.layer addAnimation:transition forKey:@"KCTransitionAnimation"];
}

#pragma mark 取得当前图片
- (UIImage *)getImage:(BOOL)isNext {
    
    if (isNext) {
        _currentIndex = (_currentIndex+1)%IMAGE_COUNT;
    } else {
        _currentIndex = (_currentIndex-1+IMAGE_COUNT)%IMAGE_COUNT;
    }
    
    NSString *imageName = [NSString stringWithFormat:@"%i",_currentIndex];
    return [UIImage imageNamed:imageName];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end
```