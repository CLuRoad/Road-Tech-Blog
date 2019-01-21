# 	UI事件传递&响应
#iOS知识小集/UI视图
## UIView和CALayer
* UIView为其提供内容，以及负责处理触摸等事件，参与响应链
* CALayer负责显示内容contents
体现了系统设计上的单一职责原则

## 事件传递与视图响应链
[image:E9860616-4240-4BB4-AE7C-7D1CC3D363F5-566-0000054F0C19D068/7E8444FF-52A3-4A20-A9ED-C18D3A1176DB.png]

*事件传递流程*
[image:F3771E67-716F-44F5-8BB4-BD70908C3B26-566-000005AD52E9D1B0/3F5773D2-0AED-4C7F-87D5-175089CE3E95.png]

*hitTest:withEvent:系统实现*
[image:EAFB1B2F-19AF-4F90-8335-53F9BF469F2D-566-000005BDED9B18A1/A49E3D7E-9654-42C3-B25E-94013DE941BF.png]

*代码实战*
[image:B6E6C25C-EDE3-430A-B6A5-012F3F7463DD-566-000006FD7841CFEE/00F2A51D-C153-4F59-94D9-43B49258B20C.png]
```
@implementation CustomButton

- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
    if (!self.userInteractionEnabled ||
        [self isHidden] ||
        self.alpha <= 0.01) {
        return nil;
    }
    
    if ([self pointInside:point withEvent:event]) {
        // 倒叙遍历当前对象的子视图
        __block UIView *hit = nil;
        [self.subviews enumerateObjectsWithOptions:NSEnumerationReverse usingBlock:^(__kindof UIView * _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            // 坐标转换
            CGPoint vonertPoint = [self convertPoint:point toView:obj];
            // 调用子视图的hittest方法
            hit = [obj hitTest:vonertPoint withEvent:event];
            // 如果找到了接受事件的对象，则停止遍历
            if (hit) {
                *stop = YES;
            }
        }];
        
        if (hit) {
            return hit;
        } else {
            return self;
        }
    } else {
        return nil;
    }
}

- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    CGFloat x = point.x;
    CGFloat y = point.y;
    
    CGFloat centerX = self.frame.size.width / 2;
    CGFloat centerY = self.frame.size.height / 2;
    
    double dis = sqrt((x - centerX) * (x - centerX) + (y - centerY) * (y - centerY));
    
    if (dis <= self.frame.size.width / 2) {
        return YES;
    } else {
        return NO;
    }
}

@end
```

*视图响应流程*
[image:44468519-95D2-49E6-B370-CC42E4F8E547-566-0000074A4691106A/1BA7AC43-07DF-4887-8C0A-037BAEDFA7DF.png]
[image:1A7BD710-9299-4D1C-B22C-B12F903973D0-566-000007721991CE4E/A3EBEC3E-ADA2-4BFF-BC21-126B98CDBD35.png]
[image:5DE89478-AC79-47B9-86BA-815CAC021C92-566-000007AB9CF12FD3/B51FB7DE-CD7D-4950-BF6B-B1F04D3922AA.png]

