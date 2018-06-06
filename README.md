# iOSInterview
iOS面试题总结

iOS面试题总结
主要参考：
[《招聘一个靠谱的iOS》面试题参考答案](https://github.com/ChenYilong/iOSInterviewQuestions)
https://github.com/liberalisman/iOS-InterviewQuestion-collection
https://github.com/liberalisman/iOS-Interview-Question-Answer
https://github.com/lzyy/iOS-Developer-Interview-Questions
https://github.com/Timhbw/iOSInterviewQuestions
https://github.com/chaoskyme/iOS-Interview-Questions


### 风格纠错
- 题目
![](https://camo.githubusercontent.com/748813155b831969a3ff5829f527b851d1b6c8a9/687474703a2f2f692e696d6775722e636f6d2f4f375a657639342e706e67)

- 答案示例：

```
	typedef NS_ENUM(NSInteger, CYLUserGender) {
	    CYLUserGenderUnknown,
	    CYLUserGenderMale,
	    CYLUserGenderFemale,
	    CYLUserGenderNeuter
	};
	
	@interface CYLUser : NSObject<NSCopying>
	
	@property (nonatomic, readonly, copy) NSString *name;
	@property (nonatomic, readonly, assign) NSUInteger age;
	@property (nonatomic, readonly, assign) CYLUserGender gender;
	
	- (instancetype)initWithName:(NSString *)name age:(NSUInteger)age gender:(CYLUserGender)gender;
	+ (instancetype)userWithName:(NSString *)name age:(NSUInteger)age gender:(CYLUserGender)gender;

```

- 说明

	1. `enum` 建议使用 `NS_ENUM` 和 `NS_OPTIONS` 宏来定义枚举类型，参见官方的 [Adopting Modern Objective-C](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html)
	2. 避免使用基本类型，建议使用`Foundation`数据类型:
	
		```
		int -> NSInteger
	   unsigned -> NSUInteger
	   float -> CGFloat
	   动画时间 -> NSTimeInterval
	   ```
   3. 无论是 MVC 模式还是 MVVM 模式，业务逻辑都不应当写在 Model 里：MVC 应在 C，MVVM 应在 VM。
   4. 如果方法表示让对象执行一个动作，使用动词打头来命名，注意不要使用 do，does 这种多余的关键字，动词本身的暗示就足够了。
   5. `Login` 是名词， `LogIn` 是动词，都表示登陆。
	6. 属性修饰符一般顺序： 原子性，读写 和 内存管理。
	7. 枚举类型的命名规则和函数的命名规则相同：命名时使用驼峰命名法，勿使用下划线命名法。
	8. `@interface` 与 `@property` 属性声明中间应当间隔一行。
	9. @interface 与 @property 属性声明中间应当间隔一行。


	[详细说明](https://github.com/ChenYilong/iOSInterviewQuestions/blob/master/01《招聘一个靠谱的iOS》面试题参考答案/《招聘一个靠谱的iOS》面试题参考答案（上）.md#1-风格纠错题0)
	
### 关于属性修饰符
- 属性修饰符定义  ARC环境下: 
 
	**原子性**  
	+ `nonatomic`：原子性访问，对属性赋值的时候不加锁，多线程并发访问会提高性能。  
	+ `atomic`：属性默认为atomic，提供多线程安全，在多线程环境下，原子操作是必要的，否则有可能引起错误的结果。  
	+ 在iOS开发中，几乎所有属性都声明为 nonatomic。 但是在开发 Mac OS X 程序时，使用 atomic 属性通常都不会有性能瓶颈。
	
	**读写**  
	+ `readwrite`：同时产生setter/getter方法。  
	+ `readonly`：只产生简单的getter,没有setter。

**引用计数(内存管理)**  
	+ `copy`：目标对象引用计数不变，拷贝一份引用计数为1的对象，该属性指向拷贝对象。  
	+ `weak`：目标对象引用计数不变，该属性指向目标对象地址，当目标对象销毁时，该属性置为nil。`Delegate`基本总是使用`weak`，以防止循环引用。自定义 IBOutlet 控件属性一般也使用 weak。  
	+ `strong`：目标对象引用计数+1，该属性指向目标对象地址。
	+ `assign`：基础数据类型(NSInteger，CGFloat)和C数据类型（int, float, double, char等）使用。	

[详细](https://github.com/ChenYilong/iOSInterviewQuestions/blob/master/01《招聘一个靠谱的iOS》面试题参考答案/《招聘一个靠谱的iOS》面试题参考答案（上）.md#2-什么情况使用-weak-关键字相比-assign-有什么不同)

[iOS属性常用关键字解析](http://www.wimhe.com/archives/39)


-  ARC下，不显式指定任何属性关键字时，默认的关键字都有哪些？

	+ 1. 对应基本数据类型默认关键字是  atomic,readwrite,assign 
	+ 2. 对于普通的 Objective-C 对象  atomic,readwrite,strong

	
- @synthesize和@dynamic分别有什么作用？
	
	1. @property有两个对应的词，一个是 @synthesize，一个是 @dynamic。如果 @synthesize和 @dynamic都没写，那么默认的就是@syntheszie var = _var;
	2. @synthesize 的语义是如果你没有手动实现 setter 方法和 getter 方法，那么编译器会自动为你加上这两个方法。
	3. @dynamic 告诉编译器：属性的 setter 与 getter 方法由用户自己实现，不自动生成。（当然对于 readonly 的属性只需提供 getter 即可）。假如一个属性被声明为 @dynamic var，然后你没有提供 @setter方法和 @getter 方法，编译的时候没问题，但是当程序运行到 instance.var = someVar，由于缺 setter 方法会导致程序崩溃；或者当运行到 someVar = var 时，由于缺 getter 方法同样会导致崩溃。编译时没问题，运行时才执行相应的方法，这就是所谓的动态绑定。



### 其他

- objc中向一个对象发送消息[obj foo]和objc_msgSend()函数之间有什么关系？

	`[obj foo];` 在objc编译时，会被转意为：`objc_msgSend(obj, @selector(foo));` 。
	
- 什么时候会报unrecognized selector的异常？

- 一个objc对象如何进行内存布局？（考虑有父类的情况）

	+ 所有父类的成员变量和自己的成员变量都会存放在该对象所对应的存储空间中.
	+ 每一个对象内部都有一个isa指针,指向他的类对象,类对象中存放着本对象的: 1 对象方法列表（对象能够接收的消息列表，保存在它所对应的类对象中）, 2 成员变量的列表, 3 属性列表。

	
-  objc中的类方法和实例方法有什么本质区别和联系？

	**类方法**：

	+ 类方法是属于类对象的
	+ 类方法只能通过类对象调用
	+ 类方法中的self是类对象
	+ 类方法可以调用其他的类方法
	+ 类方法中不能访问成员变量
	+ 类方法中不能直接调用对象方法

	**实例方法**：

	+ 实例方法是属于实例对象的
	+ 实例方法只能通过实例对象调用
	+ 实例方法中的self是实例对象
	+ 实例方法中可以访问成员变量
	+ 实例方法中直接调用实例方法
	+ 实例方法中也可以调用类方法(通过类名)

	
- _objc_msgForward是 IMP 类型，用于消息转发的：当向一个对象发送一条消息，但它并没有实现的时候，_objc_msgForward会尝试做消息转发。


- `+(void)load;`  `+(void)initialize；`有什么用处？

	在Objective-C中，runtime会自动调用每个类的两个方法。+load会在类初始加载时调用，+initialize会在第一次调用类的类方法或实例方法之前被调用。这两个方法是可选的，且只有在实现了它们时才会被调用。   
	共同点：两个方法都只会被调用一次。
	

- 简述你了解的锁

	+ 互斥锁：NSLock、pthread_mutex、@synchronized。
	
	加锁后，其他加锁操作阻塞直到解锁。
	
	+ 递归锁：NSRecursiveLock。
	
	一个线程可以多次加锁，相应的要对应多次解锁其他线程才可以加锁。
	
	+ 条件锁：NSCondition、NSConditionLock。
	
	锁满足指定条件时才继续执行，否则阻塞。
	
	+ 信号量：dispatch_semaphore。
	
	wait操作阻塞直到signal被调用。
	
	+ 读写锁：pthread_rwlock。
	
	读模式占有锁时其他线程只能读；写模式占有锁时其他线程不能进行任何操作。
	
- OC中类和结构体有什么区别？

1：类指针赋值时只是复制了地址，结构体是复制内容；
2：类不能有同名同参数个数的方法，结构体可以；
3：结构体方法实现编译时就确定了，类方法实现可动态改变；
4：内存分配不一样，结构体在栈，类在堆；
5：结构体可以多重继承，类只能单继承。

- 通知和协议的区别

一个协议一时间只能有一个代理对象，而一个通知一时间可以有多个监听者。
通知的发送和监听依靠通知中心，协议则可以自己创建，通过setDelegate指定代理对象。

- iOS内存使用注意事项和优化

使用注意事项

访问野指针：数据越界、对象已经释放但对其发送消息等；
内存泄漏：循环引用、imageNamed读取图片等；
触碰内存峰值：for循环声明变量等；
申请了不使用的内存：声明变量但未使用、只在某个逻辑分支用到某些变量但一开始就初始化等。

内存优化

访问野指针：访问前加判断；

这部分问题大多是多线程造成的，比如两个线程同时执行一个方法，方法内部对数组有更新操作。
内存泄漏：Instrument Leaks/Allocations检测、使用imageWithContentsOfFile读取图片等；

imageNamed会缓存加载的图片；imageWithContentsOfFile只是简单的加载图片。
触碰内存峰值：手动添加释放池；

for循环内大量创建局部变量，这些局部变量会等到RunLoop的下一个循环才释放，
而手动加入释放池则会提前释放。
申请了不使用的内存：懒加载。

- [iOS - XML解析](https://juejin.im/post/581f2236da2f60005d021c36)

- iOS常用设计模式


装饰模式：分类；
代理模式：协议；
工厂模式：UIButton创建；
原型模式：[object copy]；
观察者模式：KVO；
迭代器模式：数组的遍历；
单例模式：Appdelegate；
命令模式：给对象发消息；
职责链模式：事件传递链；
中介者模式：模块解耦；
解释器模式：Siri语意识别；

- AFNetWorking
AFNetworking到底做了什么？(终)

- [HTTPS 原理详解](https://blog.upyun.com/?p=1347)


### 遗留

- 什么是method swizzling?
- UIView和CALayer是啥关系？
- 最新版SDWebImage的使用
- HTTP协议总结
- 事件传递链/事件响应链
- __weak、__strong、__block
- NSTimer原理 https://www.jishux.com/plus/view-599518-1.html

NSTimer、GCD定时器和CADisplayLink。
- loadView
- frame和bounds区别
- iOS @property、@synthesize和@dynamic
- SDWebImage
- UITableView优化手段
