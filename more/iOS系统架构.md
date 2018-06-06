iOS系统架构
----

可触摸层（Cocoa Touch Layer）、媒体层（Media Layer）、核心服务层（Core Services Layer）、核心系统层（Core OS Layer）。

### 核心系统层（Core OS Layer）

![Core OS](https://upload-images.jianshu.io/upload_images/797918-2965748e8e244c2e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

操作系统内核服务（BSD sockets、I/O访问、内存申请、文件系统、数学计算等）
本地认证（指纹识别验证等）
安全（提供管理证书、公钥、密钥等的接口）
加速 (执行数学、大数字以及DSP运算,这些接口iOS设备硬件相匹配）


### 核心服务层（Core Services Layer）

![Core Services](https://upload-images.jianshu.io/upload_images/797918-cc0de0f6f45ff252.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

CFNetwork（网络访问）
Core Data（数据存储）
Core Location（定位功能）
Core Motion（重力加速度，陀螺仪）
Foundation（基础功能如NSString）
Webkit（浏览器引擎）
JavaScript（JavaScript引擎）

### 媒体层（Media Layer）
![](https://upload-images.jianshu.io/upload_images/797918-30e2f3470787b368.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

图像引擎（Core Graphics、Core Image、Core Animation、OpenGL ES）
音频引擎 （Core Audio、 AV Foundation、OpenAL）
视频引擎（AV Foundation、Core Media）


### 可触摸层（Cocoa Touch Layer）
![](https://upload-images.jianshu.io/upload_images/797918-486bd1393e7d908a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

UIKit（界面相关）
EventKit（日历事件提醒等）
Notification Center（通知中心）
MapKit（地图显示）
Address Book（联系人）
iAd（广告）
Message UI（邮件与SMS显示）
PushKit（iOS8新push机制）