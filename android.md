# Android基础知识

![Android知识图谱](http://lxzh.oss-cn-hangzhou.aliyuncs.com/markdown/androidknowledge.png)

## 基础知识

### Java基础

1. 重载与重写的区别

    **重载**和**重写**是多态性的不同体现；**重写**是子类对父类相同方法(相同方法名、参数与返回类型)的覆写，子类对象使用这个方法时将调用子类中的实现；**重载**是一个类中多个同名方法的不同实现，它们仅方法名相同，参数类型、个数不同，返回值类型也可不同(不可仅返回值类型不同)。

2. String、StringBuffer与StringBuilder的区别

    **String**是字符串常量;
    **StringBuffer**字符串变量(线程安全);
    **StringBuilder**字符串变量(非线程安全).

3. String类能否被继承？

    String类是final类,不可以被继承.

4. switch支持的数据类型有哪些？

    java switch 在 **jdk1.6**以前支持五种数据类型:`byte`、`char`、`short`、`int`、`enum`, **jdk1.7**中为6种:`byte`、`char`、`short`、`int`、`enum`、`String`.

5. final、finally与finalize的区别

    **final**用于声明属性、方法和类，分别表示属性不可变、方法不可覆盖、类不可被继承
    **finally**是异常捕获处理语句结构的一部分, 表示无论异常是否发生都会执行的
    **finalize**是Object类的一个方法，在垃圾回收器执行的时候回调被回收对象的此方法，可以覆盖此方法提供垃圾回收时的其他资源回收，例如关闭文件等。

6. ArrayList, Vector, LinkedList的存储性能和特性

    **ArrayList** 和 **Vector**都是使用数组来存储数据，此数组元素数大于实际存储的数据，能够增加和插入元素，他们都允许直接按照序号索引元素，但是插入元素涉及数组元素移动等内存操作，所以索引数据快，但插入删除数据慢; **Vector**由于使用了`synchronized`方法(线程安全)，通常性能上较ArrayList差; **LinkedList**使用双向链表实现存储，按照序号索引数据需要前向或后向遍历，但是插入数据时只需要修改当前项的前后节点即可，所以插入速度快但索引慢.

7. Collection和Collections的区别

    **Collection**是集合类的上级接口，继承于它的接口主要有Set和List;
    **Collections**是针对集合类的一个工具类，它提供了一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作.

8. sleep与wait的区别

    1) sleep来自Thread类，使用时需要捕获异常; wait来自Object类，使用时不需要捕获异常;<br/>
    2) sleep方法没有释放锁，wait方法释放了锁，使得其他线程可以使用同步控制块或者方法;<br/>
    3) sleep不出让系统资源，wait是进入线程等待池等待，出让系统资源，其他线程可以占用CPU;<br/>
    4) sleep(milliseconds)可以用时间指定使它自动唤醒过来，如果时间不到只能调用interrupt()强行打断; 一般wait不会加时间限制，因为如果wait线程的运行资源不够，再出来也没用，要等待其他线程调用notify/notifyAll唤醒等待池中的所有线程，才会进入就绪队列等待OS分配系统资源;<br/>
    5) wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用 .

9. 同步和异步有什么不同，在什么情况下使用

    如果数据需要在线程间共享，则必须进行同步存取。例如正在写的数据以后可能被另一个线程读到，或者正在读的数据可能已经被另一个线程写过了，那么这些数据就是共享数据，需要同步存储才能避免出错;<br/>
    应用程序在进行耗时操作时，为了不阻塞UI以及用户交互响应，需要采用异步编程.

10. 抽象类与接口的区别

    abstract关键字修饰的方法为抽象方法，一个类中只要有一个抽象方法，就必须用abstract定义该类，即抽象类;<br/>
    用interface修饰的类即为接口，里面的方法都是抽象方法，接口里面的字段都是公有字段，即public static final修饰的字段.

11. 接口是否可以继承接口？抽象类是否可以实现接口？抽象类是否可以继承实体类？

    接口可以继承接口;<br/>
    抽象类可以实现(implements)接口;<br/>
    抽象类可以继承实体类，但前提是实体类必须有明确的构造函数.

12. abstract的方法是否可以同时static？是否可以同时native？是否可以同时是synchronized？
https://www.cnblogs.com/guweiwei/p/6600364.html

13. 线程中wait、join、sleep、yield、notify、notifyall、synchronized的区别与联系
https://www.cnblogs.com/DarrenChan/p/5733132.html

14. 单例模式的写法
https://www.cnblogs.com/hupp/p/4487521.html
https://blog.csdn.net/javazejian/article/details/71333103

15. 自动拆装箱
https://juejin.im/post/5b8de48951882542d63b4662

16. Java中的引用类型: https://www.cnblogs.com/baizhanshi/p/6272584.html
    - 强引用
    - 弱引用
    - 虚引用
    - 软引用

17. String intern() 方法的作用
https://mp.weixin.qq.com/s/w697uPRCjn6kEzd_7u-aUQ

### Java进阶

1. 垃圾回收(GC)

    - Java虚拟机的内存区域中，程序计数器、虚拟机栈和本地方法栈三个区域是线程私有的，随线程生而生，随线程灭而灭；栈中的栈帧随着方法的进入和退出而进行入栈和出栈操作，每个栈帧中分配多少内存基本上是在类结构确定下来时就已知的，因此这三个区域的内存分配和回收都具有确定性，在这几个区域内就不需要过多的考虑回收的问题，因为方法结束或者线程结束时，内存自然就跟随回收了。而Java堆和方法区则不一样，所以垃圾回收重点关注的是堆和方法区部分的内存。

    - 垃圾区分算法:
    **引用计数法**、**可达性分析法**:
    https://blog.csdn.net/jie_sil/article/details/90745324

    - 垃圾回收算法:
    **标记清除法**、**复制法**、**标记——整理法**、**分代收集法**(年轻代、年老代、持久代)

    - GCRoot：

        * Android JVM中GC Roots的大致分类

            **Class** 由System Class Loader/Boot Class Loader 加载的类对象，这些对象不会被回收。需要注意的是其它的Class Loader实例加载的类对象不一定是 GC root，除非这个类对象恰好是其它形式的 GC root；
            **Thread 线程**，激活状态的线程；
            **Stack Local** 栈中的对象。每个线程都会分配一个栈，栈中的局部变量或者参数都是GC root，因为它们的引用随时可能被用到；
            **JNI Local** JNI中的局部变量和参数引用的对象；可能在JNI中定义的，也可能在虚拟机中定义
            **JNI Global** JNI中的全局变量引用的对象；同上
            **Monitor Used** 用于保证同步的对象，例如wait()，notify()中使用的对象、锁等。
            **Held by JVM** JVM持有的对象。JVM为了特殊用途保留的对象，它与JVM的具体实现有关。比如有System Class Loader, 一些Exceptions对象，和一些其它的Class Loader。对于这些类，JVM也没有过多的信息。

    https://blog.csdn.net/jie_sil/article/details/90745324


2. HashMap(jdk1.7与jdk1.8的区别)

    https://blog.csdn.net/woshimaxiao1/article/details/83661464

    - 介绍:HashMap是一个存储键值对(key-value)映射的散列表;

    - 内部结构:

3. 内存模型

本地方法栈 虚拟机栈 程序计数器
方法区 堆
http://www.54tianzhisheng.cn/2018/02/28/Java-Memory-Model/

4. 类的加载过程

深入理解Java类加载器(ClassLoader) https://blog.csdn.net/javazejian/article/details/73413292

5. 线程池:https://www.cnblogs.com/dolphin0520/p/3932921.html

- 基本参数

    1. keepAliveTime           线程存活时间</br>
    2. allowCoreThreadTimeOut  是否允许为核心线程设置存活时间</br>
    3. corePoolSize            核心池的大小（即线程池中的线程数目大于这个参数时，提交的任务会被放进任务缓存队列）</br>
    4. maximumPoolSize         线程池最大能容忍的线程数</br>
    5. poolSize                线程池中当前的线程数</br>

- 创建线程池方法

    1. Executors.newCachedThreadPool();          //创建一个缓冲池，缓冲池容量大小为Integer.MAX_VALUE

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

    2. Executors.newSingleThreadExecutor();      //创建容量为1的缓冲池

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

    3. Executors.newFixedThreadPool(int);        //创建固定容量大小的缓冲池

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```


6. AsyncTask

7. 注解
深入理解Java注解类型(@Annotation) https://blog.csdn.net/javazejian/article/details/71860633


### C/C++

1. 虚函数

C++ 虚函数和纯虚函数的区别:
https://www.runoob.com/w3cnote/cpp-virtual-functions.html

### 数据结构

- 链表

ArrayList与LinkedArrayList的区别

- 哈希表

- 二叉树
    前序遍历
    中序遍历
    后序遍历
    层序遍历
    树形结构打印

- 红黑树

- 图

### 常见算法

- 排序

- 查找

- 动态规划

### 面向对象思想

- 封装

- 继承

- 多态

### 开发环境

- Eclipse

- Android Studio

### Android SDK

- 版本适配
- 各大版本特性

### Activity

- 生命周期

- Activity加载原理

- Activity启动

### Service

- startService

- bindService

- IntentService

    IntentService:异步处理服务，新开一个线程：handlerThread在线程中发消息，然后接受处理完成后，会清理线程，并且关掉服务。IntentService有以下特点：
    1. 它创建了一个独立的工作线程来处理所有的通过onStartCommand()传递给服务的intents。
    2. 创建了一个工作队列，来逐个发送intent给onHandleIntent()。
    3. 不需要主动调用stopSelft()来结束服务。因为，在所有的intent被处理完后，系统会自动关闭服务。
    4. 默认实现的onBind()返回null
    5. 默认实现的onStartCommand()的目的是将intent插入到工作队列中

### BroadcastReceiver

- 静态注册

- 动态注册

- 发送广播的方式

1. 静态注册
2. 动态注册

- 广播的类型

1. 顺序广播
2. 无序广播



### ContentProvider

### View

- View 的绘制流程

- View 的事件分发机制

-
### Application

### ActionBar

### Fragment

- 生命周期

https://www.cnblogs.com/LangZXG/p/6501839.html

- 重叠问题

- 屏幕旋转复用问题

### Desktop Widget

### AndroidManifest

- 四大组件
- 启动模式
- 权限
- 主题theme
- intent-filter

## UI

### Layout

- FrameLayout
- LinearLayout
- TableLayout
- GridLayout
- RelativeLayout
- DrawerLayout
- SlidingPanelLayout
- CoordinatorLayout
- ConstraintLayout

### View

- 事件分发机制

dispatchTouchEvent->onInterceptTouchEvent(false:向子View分发 true:return)->onTouch(true)->return
dispatchTouchEvent->onInterceptTouchEvent(false:向子View分发 true:return)->onTouch(false)->onTouchEvent->Click

### Custom View

- onMeasure、onLayout、onDraw

- UNSPECIFIED、AT_MOST、EXACTLY

- 自定义属性

### RecyclerView

- ListView 与 RecyclerView 的差异

- RecyclerView 里面用到的设计模式
RecyclerView中涉及的设计模式：https://blog.csdn.net/smileiam/article/details/104756706

### Handler

- Handler的工作原理以及Handler与Looper、MessageQueue三者的关系
https://blog.csdn.net/lmj623565791/article/details/38377229

    Android Handler 异步消息处理机制的妙用 创建强大的图片加载类：https://blog.csdn.net/lmj623565791/article/details/38476887

- Handler与HandlerThread

### anim

- View Animation
1. Tween Animation
2. Frame Animation

- Proterty Animation
1. ValueAnimator
2. ObjectAnimator
3. AnimatorSet

### Resource

- res
1. anim
2. animator
3. drawable
4. mipmap
5. interpolator
6. layout
7. menu
8. raw
9. values
1) arrays
2) attrs
3) bools
4) colors
5) string
6) styles
10. xml

- assets

AssetManager

### Drawable

- AnimationDrawabble:</br>
    An object used to create frame-by-frame animations, defined by a series of Drawable objects, which can be used as a View object's background.
- AnimatedTotateDrawable:</br>
    A Drawable that can animate a rotation of another Drawable.
- RotateDrawable:</br>
    A Drawable that can rotate another Drawable based on the current level value.
- ScaleDrawable:</br>
    A Drawable that changes the size of another Drawable based on its current level value.
- TransitionDrawable:</br>
    An extension of <layer-list> that is intended to cross-fade between the first and second layer.
- BitmapDrawable:</br>
     Drawable that wraps a bitmap and can be tiled, stretched, or aligned.
- ColorDrawable:</br>
    A specialized Drawable that fills the Canvas with a specified color, with respect to the clip region
- NinePatchDrawable:</br>
    A resizeable bitmap, with stretchable areas that you define.
- GradientDrawable:</br>
    Basic method for drawing shapes via XML.
- ClipDrawable:</br>
    A Drawable that clips another Drawable based on this Drawable's current level value
- InsetDrawable:</br>
    A Drawable that insets another Drawable by a specified distance.
- LayerDrawable:</br>
    A Drawable that manages an array of other Drawables. These are drawn in array order.
- LevelListDrawable:</br>
    A resource that manages a number of alternate Drawables, each assigned a maximum numerical value.
- StateListDrawable:</br>
    Lets you assign a number of graphic images to a single Drawable and swap out the visible item based on state.

### OenGL

### 序列化

https://www.cnblogs.com/tangZH/p/10998065.html

- Serializable

- Parcelable

## 通信

### IPC

- Binder

Binder 学习指南：http://weishu.me/2016/01/12/binder-index-for-newer/

为什么Android要采用Binder作为IPC机制？
https://blog.csdn.net/gaolh89/article/details/102630238


- AIDL

- Messager

- ContentProvider

- Broadcast


### 线程同步与通信

### Socket

### 网络请求

TCP 与 UDP:
https://www.cnblogs.com/fundebug/p/differences-of-tcp-and-udp.html

TCP的三次握手与四次挥手理解:
https://blog.csdn.net/qq_38950316/article/details/81087809

### FTP

### Http

- Http

HTTP请求方式中8种请求方法:
https://www.cnblogs.com/weibanggang/p/9454581.html
HTTP请求、响应报文格式:
https://blog.csdn.net/a19881029/article/details/14002273

- Https

HTTP与HTTPS的区别:
https://www.cnblogs.com/baizhanshi/p/10282797.html
https://blog.csdn.net/gdutxiaoxu/article/details/97885526

### Bluetooth

### NFC

### Headset

### USB

### Wifi


## 数据持久化

### Sqlite

- SQLiteOpenHelper
- ContentProvider

### File

- Internal Storage
- External Storage

### SharePreferences

## 注解

## 性能

### UI 优化

- Layout Inspector

- uiautomatorviewer

### 内存优化

- OOM
- ANR
- 分析

1. Heap
2. adb shell
3. TraceView
4. Dalvik日志
5. logcat
6. MAT
7. LeakCanary
8. Profiler

### 电量优化

Battery Historian https://www.jianshu.com/p/27ba2759b221/

### 流量优化

## 编译

### JVM、Dalvik与Art的区别

https://blog.csdn.net/evan_man/article/details/52414390

JVM执行的是.class文件，JVM基于虚拟机栈的虚拟机
Dalvik执行的是.dex文件，Dalvik基于寄存器的虚拟机
Art执行的是应用安装时编译的机器码

## 调试

### Logcat

### adb

- 命令行控制手机

```shell
#进入手机交互模式
adb shell
#命令控制输入文本
input text 123456
#命令控制输入按键值 keycode可百度查
input keyevent 7
input keyevent KEYCODE_1
input keyevent KEYCODE_HOME
#点击坐标(123,345)
input tab 123 345
#从坐标(1,2)滑动到(100,300),200毫秒内
input swipe 1 2 100 300 200

#截图
adb shell screencap /sdcard/1.png
adb shell screencap -p /sdcard/1.png

#录屏 只能使用 **Ctrl+C** 停止
adb shell screenrecord /sdcard/1.mp4
#下载手机文件到本地
adb pull /sdcard/1.mp4 .

#wifi调试
adb shell setprop service.adb.tcp.port 5555
adb shell ifconfig | grep addr
adb shell stop adbd && start adbd
adb connect xx.xx.xx.xx:5555

#电量
adb shell dumpsys batterystats --enable full-wake-history 
adb shell dumpsys batterystats --reset
adb shell dumpsys batterystats

#查看安装的应用
adb shell pm list packages


```

### HierachyViewer

### TraceView

### Heap

### Lint


## 适配

### OS Version

### Screen Size

### Screen px


## 测试

### Monkey

### MonkeyRunner

### JUnit

### Robotium

### Appium

### Athrun(TMTS)

### UIAutomator


## 安全

### 服务器安全

### 通信安全

### 数据加密

### 数据验签

### 代码混淆

### 应用加固

### webview/JS安全调用

### MD5、DES、RSA、https、证书、权限

- [https](https://juejin.im/entry/5b5fc811f265da0f697049c1)
http三次握手 四次挥手


## NDK

### JNI

### C语言

### C++

- 引用与指针

- 智能指针

- 虚函数

- 抽象类

- 继承与多态

- 面向对象

- 模板

## 手机功能

### 电话

- 联系人
- 通话记录

### 短/彩信

### Camera

### Audio

### SD卡

### 感应器

- 加速
- 方向
- 重力
- 光线
- 陀螺仪
- 磁场
- 距离
- 温度
- 压力
- 线性加速度
- 旋转
- 多点触控

## 第三方扩展

### 地图

### 语音识别

### 短信验证

### 支付

### 统计分析

### 广告

## 插件化

### VirtualAPK

### DroidPlugin

### RePlugin

### Tinker

### AndFix

### Robust

### Shadow

...


## 其他

### Intent

### wifi

### 国际化

### PopuWindow


## Hook

### 动态代理

### CGLib

### Dexmaker

### Lancet

### epic

### SandHook

### xHook

### Frida

### Inline Hook
