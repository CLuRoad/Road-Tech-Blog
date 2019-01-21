[XXXX](#jump)

<p style="background-color:rgb(255,255,0)">
通过 rbg 值设置背景颜色
</p>


# 真题

* *使用masonry时遇到的问题【阿里Lazada】*
[使用Masonry(AutoLayout)出现约束冲突的解决方案 - 简书](https://www.jianshu.com/p/611fee0c2a60)
	1. masonry设置优先级，当两个约束产生冲突时，iOS会弃用优先级低的约束，而选择优先级较高的约束，达到正确布局的效果。`make.width.equalTo(view1).priorityLow();`
	2. `UIView`的contentHugging（不想变大约束）和contentCompressionResistance （不想变小约束）

* *App启动耗时优化【阿里Lazada】*
[iOS一次立竿见影的启动时间优化 - CocoaChina_让移动开发更简单](http://www.cocoachina.com/ios/20170816/20267.html)
计时器profile工具
main前1s
main后4s降到800ms

第一类：日志、统计、网络
第二类：用户数据
第三类：直播、高德地图等业务代码

第一个页面渲染：
第一个页面渲染的优化思路就是，先立马展示一个空壳的 UI 给用户，然后在 viewDidAppear 方法里进行数据加载解析渲染等一系列操作，工作台页面

* *对runtime的理解【阿里Lazada】*
Runtime是一套底层纯C语言API，OC代码最终都会被编译器转化为运行时代码，通过消息机制决定函数调用方式，这也是OC作为动态语言使用的基础

* *编译优化【头条】*

* *@selector的数据结构【头条】*
方法以 selector 作为索引. selector 的数据类型是 SEL. 虽然 SEL 定义成 char*, 我们可 以把它想象成 int. 每个方法的名字对应一个唯一的 int 值.比如, 方法 addObject: 可能 对应的是 12. 当寻找该方法是, 使用的是 selector,而不是名字 @"addObject:"

* *block什么时候用strongSelf【头条】*
在 block 中先写一个 strong self，其实是为了避免在 block 的执行过程中，突然出现 self 被释放的尴尬情况。通常情况下，如果不这么做的话，还是很容易出现一些奇怪的逻辑，甚至闪退。

* *七层网络，ping命令在那一层【头条】*
 7 应用层 6 表示层 5 会话层 4 传输层 3 网络层 2 数据链路层 1 物理层
ping命令基于ICMP(控制报文协议)，是在网络层（ping命令是应用层的命令，ICMP在网络层），而端口号是传输层的内容，在ICMP根本不关心这样的东西

* *UDP和TCP怎么选择，王者荣耀采用哪一种【头条】*
[TCP与UDP的选择—结合QQ来说明 - 一缕阳光的博客 - CSDN博客](https://blog.csdn.net/zgaoq/article/details/74011907)

* *runtime解决什么难题*
	1. 按钮重复点击问题
	用运行时和分类，替换`UIControl`响应事件`sendAction:to:forEvent:`
	2. 分类中使用属性

* *Crash率*
[如何将APP崩溃率降低到万分之一以下 - CocoaChina_让移动开发更简单](http://www.cocoachina.com/ios/20170516/19272.html)
bugly
MLeaksFinder
内存管理（block、NSTimer、通知、KVO）
容错处理

* *项目里哪些地方用到消息转发，有没有Invocation类*
只知道method的nameString，且有多个参数时，使用Invocation
比如给UISearchBar 设置leftPlaceHolder
```
@implementation UISearchBar (Extension)
- (void)setLeftPlaceholder:(NSString * _Nullable)placeholder {
    
    SEL centerSelector = NSSelectorFromString([NSString stringWithFormat:@"%@%@", @"setCenter", @"Placeholder:"]);
    if ([self respondsToSelector:centerSelector]) {
        BOOL centeredPlaceholder = NO;
        NSMethodSignature *signature = [[UISearchBar class] instanceMethodSignatureForSelector:centerSelector];
        NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:signature];
        [invocation setTarget:self];
        [invocation setSelector:centerSelector];
        [invocation setArgument:&centeredPlaceholder atIndex:2];
        [invocation invoke];
        
    }
}
@end
```

* *什么时候会调用dealloc*
1、这个类被release的时候会被调用；
2、这个对象的retain count为0的时候会被调用；
或者说一个对象或者类被置为nil的时候；

* *NSObject有哪些方法*

```
// 加载
+ (void)load;

// 初始化
+ (void)initialize;
- (instancetype)init

// 创建
+ (instancetype)new ;
+ (instancetype)allocWithZone:(struct _NSZone *)zone ;
+ (instancetype)alloc ;
- (void)dealloc ;

- (void)finalize ;

// 对象拷贝
- (id)copy;
- (id)mutableCopy;
+ (id)copyWithZone:(struct _NSZone *)zone;
+ (id)mutableCopyWithZone:(struct _NSZone *)zone;

// 消息转发相关
+ (BOOL)instancesRespondToSelector:(SEL)aSelector;
+ (BOOL)conformsToProtocol:(Protocol *)protocol;
- (IMP)methodForSelector:(SEL)aSelector;
+ (IMP)instanceMethodForSelector:(SEL)aSelector;
- (void)doesNotRecognizeSelector:(SEL)aSelector;

- (id)forwardingTargetForSelector:(SEL)aSelector ;
- (void)forwardInvocation:(NSInvocation *)anInvocation ;
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector;

+ (NSMethodSignature *)instanceMethodSignatureForSelector:(SEL)aSelector ;

- (BOOL)allowsWeakReference;
- (BOOL)retainWeakReference;

// 类操作
+ (BOOL)isSubclassOfClass:(Class)aClass;

+ (BOOL)resolveClassMethod:(SEL)sel ;
+ (BOOL)resolveInstanceMethod:(SEL)sel ;

+ (NSUInteger)hash;
+ (Class)superclass;
+ (Class)class;

// 调试信息
+ (NSString *)description;
+ (NSString *)debugDescription;
```

* *NSObject的 hash*
一个对象在用作key值时，其 hash 方法会被调用，用以生成一个唯一标识符，NSDictionary 需要根据唯一 key 值（根据 hash 算法生成的值）查找对象， NSSet 需要根据 hash 值来确保过滤掉重复的对象。

* *NSNumber能不能做NSDictionary的key，自定义的对象呢*
NSNumber、NSArray都可以做为key，自定义的类型需要遵守`NSCopying`协议，实现`- (id)copyWithZone:(NSZone *)zone`方法和`- (BOOL)isEqual:(MyClass *)object`方法

* *消息转发机制，那三个方法调用结束后调用哪个方法，还是直接Crash*
`- (void)doesNotRecognizeSelector:(SEL)aSelector`
可以在父类里使用次方法，外部无法调用，子类实现

* *load和initialize，在load方法里做什么*

	1.  load和initialize方法都会在实例化对象之前调用，前者在main函数之前调用，后者在之后调用
	2.  load和initialize方法都不用显式地调用父类的方法而是自动调用，即使子类没有initialize方法也会调用父类的方法，而load方法则不会调用父类。
	3.  load方法通常用来进行Method Swizzle，initialize方法一般用于初始化全局变量或静态变量。
	4.  *load和initialize方法内部使用了锁*，因此它们是线程安全的。实现时要尽可能保持简单，避免阻塞线程，不要再使用锁。


* *查找两个数组里重复的数字*
各自排序、去重
合并、查重

* *数组去重*
先排序再去重

* *Pods私有库*

* *二叉树*



#面试题





<span id="jump">Hello World</span>
