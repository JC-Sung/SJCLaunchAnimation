# SJCLaunchAnimation
不用写一行代码，让你的APP启动的时候动画展现

```
#import <Foundation/Foundation.h>

@interface LaunchAnimation : NSObject

@end

@implementation LaunchAnimation

+ (void)load{
    [self shareManager];
}

+(LaunchAnimation *)shareManager{
    static LaunchAnimation *instance = nil;
    static dispatch_once_t oneToken;
    dispatch_once(&oneToken,^{
        instance = [[self alloc] init];
    });
    return instance;
}

- (instancetype)init{
    self = [super init];
    if (self) {
        [[NSNotificationCenter defaultCenter] addObserverForName:UIApplicationDidFinishLaunchingNotification object:nil queue:nil usingBlock:^(NSNotification * _Nonnull note) {
            static dispatch_once_t oneToken;
            dispatch_once(&oneToken,^{
                [self showVCWithAnimation];
            });
        }];
    }
    return self;
}

- (void)showVCWithAnimation{
    [UIApplication sharedApplication].keyWindow.transform = CGAffineTransformMakeScale(1.2, 1.2);
    //    [UIView transitionWithView:[UIApplication sharedApplication].keyWindow duration:0.25 options:UIViewAnimationOptionCurveEaseIn animations:^{
    //        [UIApplication sharedApplication].keyWindow.transform = CGAffineTransformMakeScale(1.0, 1.0);
    //    } completion:^(BOOL finished) {
    //
    //    }];
    
    [UIView animateWithDuration:0.5 delay:0.f usingSpringWithDamping:0.9 initialSpringVelocity:0.1 options:UIViewAnimationOptionCurveEaseIn animations:^{
        [UIApplication sharedApplication].keyWindow.transform = CGAffineTransformMakeScale(1.0, 1.0);
    } completion:^(BOOL finished) {
        
    }];
}


@end

