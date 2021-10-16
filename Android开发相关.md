﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿[TOC]



# 知识点

## 概念

- JNI：Java Native Interface
- NDK：Native Develop Kit 使用原生代码语言（C、C++）开发

四大组件：Activity、BroadcastReceiver、Content Provider 、Service

想要更新应用程序里的UI元素，必须在`主线程`中进行，否则就会出现异常  

#### 系统架构

![](Android开发相关.assets/format,f_auto.jpeg)

- 应用层： 核心应用程序，如电子邮件
- 应用框架层：提供大量API，一系列服务和系统
- 系统运行库层：函数库、运行库：Dalvik虚拟机          Android运行时：Android核心库集、ART
- Linux内核层



##### Gradle执行顺序：

首先执行项目目录下面的setting.gradle
然后执行项目目录下面的build.gradle
然后根据setting.gradle中的配置顺序逆序执行module的build.gradle(即后写的先执行)

##### Android虚拟机

- Java虚拟机(JVM)：运行的是 class文件、基于栈、可能会有 冗余字符串常量
- Dalvik虚拟机(DVM)：Java->class文件->Dalvik字节码->打包到dex中->DVM通过解释DEX文件来执行这些字节码、基于寄存器

##### 调试打印

在onCreate（）方法外面输入logt，然后按下TAB键，这时就会以当前的类名作为值自动生成一个TAG变量，然后在输入logd，按TAB生成级别为debug的打印日志

（有5种打印级别，在logcat查看、过滤）   tag一般是当前类名   自定义过滤器是 在tag中过滤。还有关键字过滤

- v
- d   调试
- i   
- w   警告
- e  错误

##### HTTPDNS

不走传统的DNS解析，而是自己搭建基于HTTP协议的DNS服务器集群，分布在多个地点和多个运营商，当客户端需要DNS解析的时候，直接通过HTTP协议(通过客户端sdk和服务端)进行请求这个服务器集群，获得就近的地址，实现智能调度。

- 传统的DNS有解析慢，更新不及时，转发跨运营商，nat跨运营商等问题，影响了流量的调度。

##### MMKV

基于 mmap 内存映射的 key-value 组件，底层序列化/反序列化使用 protobuf 实现，性能高，稳定性强。

## 控件



### EditText

设置文本：setText`在这里插入代码片`
```bash
//获取文本
EditText editText1 =(EditText) findViewById (R.id.editText1);
c=Integer.parseInt(editText1.getText().toString());
```
### 最好使用ImageView，而不是ImageButton

ImageView：

```
android:layout_height="100dp"
android:scaleType="centerCrop"   
```

scaleType属性：指定图片的缩放模式。由于各张水果图片的长宽比例可能会不一致，为了让所有的图片都能填充满整个ImageView，这里使用了centerCrop模式，它可以让图片保持原有比例填充满ImageView，并将超出屏幕的部分裁
剪掉。  

展示图片 可以 使用 Glide： 开源图片加载库，不只是本地图片，也可以是 gif文件，网络图片。(还可以 图片压缩等功能)

### 最好使用Recyclerview，而不是ListView

### ListView: 

实现方式： 1. xml引入listview，使用array.xml的String 2. activity继承 ListActivity，使用ArrayAdapter

### 选项卡：

tablehost 里面套 tablewidget 套framelayout

### 列表选择框：

spinner：entries属性设置文本信息

### CheckBox

```java
//CheckBox监听器
checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                if (isChecked) {
                    Toast.makeText(context, tv.getText().toString(), Toast.LENGTH_SHORT).show();
                }
            }
        });
```

## RecyclerView

有4级缓存

一级缓存：mAttachedScrap、mChangedScrap

二级缓存：mCacheViews

三级缓存：mViewCacheExtension

四级缓存：mRecyclerPool

一级缓存为屏幕内缓存，二级缓存为屏幕外缓存，三级缓存为自定义缓存，四级缓存为缓存池缓存。一二三即缓存直接不需要重新绑定View，四级缓存需要绑定Holder设置数据。禁用缓存可以使用ViewHolder的setIsRecyclable方法。

## Activity

### 4种状态和生命周期

4种状态： running、paused（可见但失去焦点）、stopped（不可见）、killed（被杀掉，移出堆栈中）

生命周期: onCreate() - onStart() （onRestart（））  - onResume(准备与用户交互)   - onPause()  - onStop()  - onDestroy() 

![image-20210727184233446](Android开发相关.assets/image-20210727184233446.png)

### 页面跳转与关闭

- 跳转

```bash
// 发送
ImageButton bn_edit = (ImageButton)view.findViewById(R.id.imageButton_edit);
            bn_edit.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    Intent intent=new Intent();
                    Bundle bundle=new Bundle();
                    bundle.putSerializable("inform",inform);//序列化对象
                    intent.putExtras(bundle);
                    intent.setClass(getContext(),editinform.class);
                    //启动
                    getContext().startActivity(intent);
                    ((Activity)getContext()).finish();  //finish();
             
                }
            });

//接收
        Intent intent=getIntent();
        Bundle bundle = i.getExtras();
        bundle.getString("extras",default)//获取指定字符串的extras，若为空则指定默认值
        userName =intent.getStringExtra("userName");   
       
       //获取传入的medicine对象
        medicinedemo = (medicine) getIntent().getSerializableExtra("Medicine");



```

- 直接关闭activity :`finish()；`
-  在adapter里面关闭当前的activity

```bash
//从不是activity获取上下文后 在关闭
((Activity)getContext()).finish();
```

### 4种启动模式

- 标准 默认模式 ：

  默认创建一个新的实例，`可以有多个相同的实例，也允许多个相同Activity叠加。`

- singleTop：

  ``可以有多个实例`，如果在任务的栈顶正好存在该Activity的实例， 就重用该实例（调用其onNewIntent方法），否者就会创建新的实例并放入栈顶(即使栈中已经存在该Activity实例)

- singleTask：

  `只有一个实例`。如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的onNewIntent())。重用时，会让该实例回到栈顶，因此`在它上面的实例将会被移除栈`。如果栈中不存在该实例，将会创建新的实例放入栈中。  

- singleInstance：

  `只有一个实例`，`共享activity实例`。在一个新栈中创建该Activity实例，并让多个应用共享改栈中的该Activity实例。一旦该模式的Activity的实例存在于某个栈中，任何应用再激活改Activity时都会重用该栈中的实例。

### 知道当前activity

继承一个  可以输出 类名 的BaseActivity类

###   直接退出，全部回收 activity

使用  `单例类ActivityCollector`  对activity进行统一管理



### 序列化

- 继承 Serializable： 转换成 可存储或可可传输的状态。
- 继承Parcelable  ：将一个完整对象分解成intent支持的数据类型，

## Fragment

是 activity的片段，是为了解决大屏幕

activity 调用FragmentManager的findFragmentById()方法，可以在Activity中得到相应Fragment的实例  

在每个Fragment中都可以通过调用getActivity()方法来得到和当前Fragment相关联的Activity实例  

add、remove、replace

传递数据：

activity -> fragment: 调用 fragment的setArguments(Bundle bundle)

fragment  -> activity：在 fragment中 定义 内部回调接口，让 activity 实现该 回调接口



FragmentTransaction： 事务、 commit提交

### 生命周期

也是有 4种生命周期

- 运行： activity在运行时，该fragment也处于 运行状态
- 暂停： activity在暂停（另一个未占满屏幕的Activity被添加到了栈顶  ）时
- 停止
- 销毁

单独 的回调函数：

onAttach()：当Fragment和Activity建立关联时调用。
onCreateView()：为Fragment创建视图（加载布局）时调用。
onActivityCreated()：确保与Fragment相关联的Activity已经创建完毕时调用。
onDestroyView()：当与Fragment关联的视图被移除时调用。
onDetach()：当Fragment和Activity解除关联时调用。  

![](Android开发相关.assets/image-20210812155848547-16287551302921.png)



### 动态添加Fragment

(1) 创建待添加Fragment的实例。
(2) 获取FragmentManager，在Activity中可以直接调用getSupportFragmentManager()方法获取。
(3) 开启一个事务，通过调用beginTransaction()方法开启。
(4) 向容器内添加或替换Fragment，一般使用replace()方法实现，需要传入容器的id和待添加的Fragment实例。
(5) 提交事务，调用commit()方法来完成  

### 返回栈

在事务提交之前调用了FragmentTransaction的addToBackStack()方法，它可以接收一个名字用于描述返回栈的状态，一般传入null即可 。

## BroadcastReceiver

广播分为2种

- 标准广播： 完全`异步`执行的广播  ，其他receiver几乎同时收到，但也无法被截断    sendBroadcast(intent)  

-  有序广播： `同步`执行 ，同一时刻只会有一个receiver能够收到广播消息。`有先后顺序`，前面的截断了，后面的就收不到了      sendOrderedBroadcast(intent, null)    给接受者 设置优先级  先获取的可以 abortBroadcast()  截断

不允许 开启线程

### 注册

- 动态注册： 要记得注销  自由灵活；但 `程序启动后`才能接收
- 静态注册： `不用程序启动就能接受。`在安卓8.0 之后，所有隐式广播（没有具体指定的发送 给哪个广播）都不许 使用静态 注册的方式 

使用动态广播 实现强制下线 功能

## ContentProvider

`在不同的应用程序之间实现数据共享`的功能，它提供了一套完整的机制，允许一个程序访问另一个程序中的数据，同时还能保证被访问数据的安全性  。   优点：`只选择对哪一部分数据进行共享，防止泄漏隐私`

- 使用现有的 ContentProvider读取和操作相应程序中的数据
- 自定义ContentProvider，给程序的数据提供外部访问接口

通过Context中的getContentResolver()方法获取该类的实例。  然后用于增删改查。使用Uri参数（authority和path  ）

使用ContentResolver来访问

## Service

 Service(服务）是能够在后台长时间执行运行操作并且不提供用户界面的应用程序组件。其他应用程序组件能启动服务，并且即便用户切换到另一个应用程序，服务还是可以在后台运行。

Service并不是运行在一个独立的进程当中的，而是依赖于创建Service时所在的应用程序进程。当某个应用程序进程被杀掉时，所有依赖于该进程的Service也会停止运行。

分类： 

- 启 动  调用者退出、服务依然运行
- 绑定

![](Android开发相关.assets/image-20210809165156358-16284991177561.png)

startService(intent);  //启动

stopService(intent);  // 停止

从Android 8.0系统开始，应用的后台功能被大幅削减。现在只有当应用保持在前台可见状态的情况下，Service才能保证稳定运行，一旦应用进入后台之后，Service随时都有可能被系统回收。  

如果真的非常需要长期在后台执行一些任务，可以使用`前台Service`或者`WorkManager`  

### 前台Service

前台Service和普通Service最大的区别就在于，它一直会有一个`正在运行的图标在系统的状态栏显示`，下拉状态栏后可以看到更加详细的信息，非常类似于通知的效果  。

修改Service子类的 onCreate()方法，创建通知，然后 startForeground() 让 service变成前台service。

### IntentService

执行完毕后自动停止  

## Intent

组件之间通信的载体。  可以启动一个activity、service、发送一条广播

- 显式：明确启动或触发 的组件的 类名
- 隐式： 只是指定 需要 启动或触发 的组件 应满足怎样的条件。 需要解析 ，IntentFilter来指明自己所满足的条件





## 权限

- 普通权限： 不会直接威胁到用户的安全和隐私的权限  ，系统自动授权
- 危险权限：可能会触及用户隐私或者对设备安全性造成影响的权限，如获取设备联系人信息、定位设备的地理位置等  ，`要用户手动授权` （要动态申请）

### 动态申请权限

```java
//apk 6.0 (API23)后需要动态申请权限  不需要一次授权所有申请的权限，在使用过程进行授权
//获取系统是否被授予该种权限 使用的是权限名，若同意则同组的 其他权限也被自动授权
if (ActivityCompat.checkSelfPermission(MainActivity.this, Manifest.permission.READ_CONTACTS)!= PackageManager.PERMISSION_GRANTED){
    ActivityCompat.requestPermissions(MainActivity.this,new String[]{Manifest.permission.READ_CONTACTS},1);
}
```

## View



## Adapter

#### ListView 与Adapter绑定，实现展示列表，使用searchview，实现实时搜索

### Adapter更改构造方法传参，一个类里面可能有list也要继承

## Notification

Android 8 引入： 创建通知渠道。 用户可以控制 通知渠道的重要程度等。

NotificationManager对通知进行管理  

（进行版本判断）使用NotificationChannel类构建一个通知渠道，并调用NotificationManager的createNotificationChannel()方法完成创建  （参数：渠道ID、渠道名称以及重要等级（初始的）这3个参数  ）


AndroidX库中提供了一个NotificationCompat类（兼容API），使用这个类的构造器创建Notification对象（Builder构造），就可以保证我们的程序在所有Android系统版本上都能正常工作了  。

若想 实现 点击通知后的 动作： 通过`PendingIntent`构建一个延迟执行的“意图”，当用户点击这条通知时就会执行相应的逻辑。  

setStyle()方法：可以构建出富文本的通知内容  （如长文字、大图）

## 异步

### handler

异步消息处理  机制：

![](Android开发相关.assets/webp)

- Handler：消息的发起者，不仅仅是能过将子线程的数据发送给主线程，它适用于任意两个线程之间的通信。

- looper：消息的遍历者

通信过程：（主线程向子线程发送消息时，系统自动调用prepare() ）

> 1. 调用Looper.prepare() : 一个线程 只有1个looper、MessageQueue
>
>    1. 创建Looper对象
>    2. 创建MessageQueue对象，并让Looper对象持有
>    3. 让Looper对象持有当前线程
>
> 2. 创建Handler对象  （可以有多个）
>
>    1. 得到当前线程的Looper对象，并判断是否为空
>    2. 让创建的Handler对象持有Looper、MessageQueue、Callback的引用
>
> 3. 调用Looper.loop()
>
>    死循环不断 取出message，没有消息时，next方法会阻塞。

### AsyncTask

继承AsyncTask（参数，进度单位，返回值类型）

- onPreExecute()  : 任务开始前的 初始化操作
- 在doInBackground()方法中执行具体的耗时任务 (调用 publishProgress ()进行反馈)
- 在onProgressUpdate()方法中进行UI操作
- 在onPostExecute()方法中执行一些任务的收尾工作  

执行： .execute()

## 网络

### 使用HttpURLConnection

### 使用OkHttp

开源，更好用

## 时间表达

- Date日期类
- Time时间类
- Datetime日期时间类
- Timestamp类（带时区）
  在用DateTimematter格式化时间，时间也可以直接用字符串表示

- 数据库存储：
 oracle：使用Datetime存储
  SQlite：使用integer建表存储时间戳，用new Date().getTime()存入，取出时，再转为Date类型Date date = new Date(cursor.getLong(i));

- 参考：
```bash
//对Android手机系统日历数据增删改查操作
https://blog.csdn.net/wenzhi20102321/article/details/80644833?utm_medium=distribute.pc_relevant_bbs_down.none-task-blog-baidujs-1.nonecase&depth_1-utm_source=distribute.pc_relevant_bbs_down.none-task-blog-baidujs-1.nonecase

//计算两个时间戳的差值，以字符串的形式表示
public static String getDistanceTime(long  time1,long time2 ) {
        long day = 0;
        long hour = 0;
        long min = 0;
        long sec = 0;
        long diff ;
        String flag;
        if(time1<time2) {
            diff = time2 - time1;
            flag="前";
        } else {
            diff = time1 - time2;
            flag="后";
        }
        day = diff / (24 * 60 * 60 * 1000);
        hour = (diff / (60 * 60 * 1000) - day * 24);
        min = ((diff / (60 * 1000)) - day * 24 * 60 - hour * 60);
        sec = (diff/1000-day*24*60*60-hour*60*60-min*60);
        if(day!=0)return day+"天"+flag;
        if(hour!=0)return hour+"小时"+flag;
        if(min!=0)return min+"分钟"+flag;
        return "刚刚";
    }
```
#### 使用TimePicker、DatePicker
参考：`https://blog.csdn.net/qq_37217804/article/details/80538209`

autolink：是否将指定格式的文本转换为可单击的超链接形式

## 布局

- 相对布局（RelativeLayout）:

```
android:ignoreGravity=""   设置忽略布局的组件
内部类：RelativeLayout.LayoutParams
```

- 线性布局（LinearLayout）:

- 表格布局（TableLayout）:

​	  列可以隐藏、伸缩

```
android:collapseColumns="0"   设置隐藏
android:shrinkColumns="0"        设置收缩 
android:stretchColumns="0"        设置拉伸
```

- 绝对布局（AbsoluteLayout）:

- 帧布局（FrameLayout）:

​	组件相互覆盖

```
android:foreground="@drawable/background"  设置背景
android:foregroundGravity="top|center"
```

- 约束布局

**android:ems = "10"** ：控件宽度设为10个字符的宽度

gravity：容器内（里面内容）的对齐方式

layout_gravity: 该控件所在的对齐方式（自身在布局中）

### 限定符

![image-20210812175207244](Android开发相关.assets/image-20210812175207244.png)

可以动态加载布局，如 大屏幕会加载 res/layout-large/  下的xml

使用最小宽度限定符（smallest-width-qualifier）来具体 判断。  



####  最好使用Relativelayout（尺寸适应性强），LinearLayout（方便对齐）、不要随意自己拉框，框架套框架

#### 页眉页脚

```cpp
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout 
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:tools="http://schemas.android.com/tools"
  android:id="@+id/container"
  android:layout_width="match_parent"
  android:layout_height="match_parent" >
 
  <!-- Header aligned to top -->
  <RelativeLayout
    android:id="@+id/header"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_alignParentTop="true"
    android:background="#AFA7EF"
    android:gravity="center">
 
    <TextView
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_margin="5dp"
      android:text="Header"
      android:textColor="#000000"
      android:textSize="20sp" />
  </RelativeLayout>
 
  <!-- Footer aligned to bottom -->
  <RelativeLayout
    android:id="@+id/footer"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_alignParentBottom="true"
    android:background="#6AED83"
    android:gravity="center">
 
    <TextView
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_margin="5dp"
      android:text="Footer"
      android:textColor="#000000"
      android:textSize="20sp" />
  </RelativeLayout>
 
  <!-- Content below header and above footer -->
  <RelativeLayout
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:layout_above="@id/footer"
    android:layout_below="@id/header"
    android:gravity="center">
 
    <TextView
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Content goes here"
      android:textColor="#FFFFFF"
      android:textSize="20sp" />
  </RelativeLayout>
 
</RelativeLayout>
```


```bash
https://blog.csdn.net/jasoncol_521/article/details/12556473
```

## 设计模式

### MVC



优点：Activity模块化程度高、观察者模式 可以做到多视图 同时更新

缺点：activity与界面 联系紧密，测试困难。 view无法组件化

### MVP

优点：便于测试、view可以组件化

缺点：Presenter比较笨重

### MVMM

优点：可维护性高

缺点：比较复杂

## Material Design

全新的界面设计语言   Material 库 立体设计

### Toolbar

标题栏

### 滑动菜单

菜单选项隐藏起来，通过滑动将菜单显示出来

#### DrawerLayout

#### NavigationView

两个子控件： menu(显示菜单项) 和 headerLayout (显示头部文件)

CircleImageView  ：将图片圆形化的控件

### 悬浮按钮和可交互提示

FloatingActionButton  ：悬浮按钮

Snackbar：额外增加一个按钮的点击事件  

```kotlin
fab.setOnClickListener { view ->
	Snackbar.make(view, "Data deleted", Snackbar.LENGTH_SHORT)
		.setAction("Undo") {
			Toast.makeText(this, "Data restored", Toast.LENGTH_SHORT).show()
		}
		.show()
}
```

CoordinatorLayout(加强版的FrameLayout)：可以监听其所有子控件的各种事件，并自动帮助我们做出最为合理的响应。 
比如Snackbar出现后将悬浮按钮上移.

### 卡片式布局

#### MaterialCardView

#### AppBarLayout

AppBarLayout实际上是一个垂直方向的LinearLayout，它在内部做了很多滚动事件的封装 。

### 下拉刷新

#### SwipeRefreshLayout  

把想要实现下拉刷新功能的控件放置到SwipeRefreshLayout中，就可以迅速让这个控件支持下拉刷新。  

### 可折叠式标题栏

#### CollapsingToolbarLayout

CollapsingToolbarLayout是一个作用于Toolbar基础之上的布局  。更华丽。 内部有 别的控件 + Toolbar

但CollapsingToolbarLayout是不能独立存在的，它在设计的时候就被限定只能作为AppBarLayout的直接子布局来使用。而AppBarLayout又必须是CoordinatorLayout的子布局  



## Jetpack

开发组件工具集

组件通常是定义在AndroidX库当中的，并且拥有非常好的向下兼容性。  

主要由`基础、架构、行为、界面`这4个部分组成 。    关注的是 `架构组件`。

### ViewModel

mvvm

![image-20210819181015930](Android开发相关.assets/image-20210819181015930.png)

`专门存放与界面相关的数据。`ViewModel的生命周期和Activity不同，它可以保证在手机屏幕发生旋转的时候不会被重新创建，只有当Activity退出的时候才会跟着Activity一起销毁。因此，将与界面相关的变量存放在ViewModel当中  

![image-20210825141042757](Android开发相关.assets/image-20210825141042757.png)

比较好的编程规范是给每一个Activity和Fragment都创建一个对应的ViewModel。

通过ViewModelProvider来获取ViewModel的实例 。ViewModel有其独立的生命周期，并且其生命周期要长于Activity。  

使用 ViewModelProvider.Factory  来 向ViewModel 传递参数

### Lifecycles

在一个非Activity的类中去感知Activity的生命周期  

方法：

- 在Activity中嵌入一个隐藏的Fragment来进行感知
- 手写监听器的方式来进行感知  
- 实现LifecycleObserver接口 ，使用注解 @OnLifecycleEvent () ,借助LifecycleOwner.addObserver(observer) 

### LiveData

响应式编程组件 ,可以包含任何类型的数据，并在数据发生变化的时候通知给观察者。LiveData特别适合与ViewModel结合在一起使用 . 

使用MutableLiveData对象，getValue()方法用于获取LiveData中包含的数据；setValue()方法用于给LiveData设置数
据，但是只能在主线程中调用；postValue()方法用于在非主线程中给LiveData设置数据。  

主线程使用 observe() 进行观察

转换方法：

- map()：将实际包含数据的LiveData和仅用于观察数据的LiveData进行转换。 （转型成任意其他类型的LiveData  ） 
- switchMap()方法  ：ViewModel中的某个LiveData对象是调用另外的方法获取的，将这个LiveData对象转换成另外一个可观察的LiveData对象  

### Room

ORM框架，组成：Entity、Dao和Database

@Query("sql")：需要编写具体的SQL语句 @Insert、@Delete、@Update 则不用

### WorkManager

处理一些要求`定时执行`的任务，它可以根据操作系统的版本自动选择底层是使用AlarmManager实现还是JobScheduler实现，从而降低了我们的使用成本。另外，它还支持周期性任务、链式任务处理等功能，是一个非常强大的工具。  

> 步骤：
>
> (1) 定义一个后台任务，并实现具体的任务逻辑；
> (2) 配置该后台任务的运行条件和约束信息，并构建后台任务请求；
> (3) 将该后台任务请求传入WorkManager的enqueue()方法中，系统会在合适的时间运行。  



# 连接云服务器 数据库

购买云主机后，ssh    用户名@IP地址，输入密码就可以远程登陆腾讯云上的机器，安装mysql，创建用户名和数据库，授权访问。也可以命令 mysql -u 数据库用户 -h IP地址 -p，然后输入数据库用户密码操作数据库



注：MySQL数据对ip有权限要求，默认只允许127.0.0.1本机访问，需要在配置文件中改。服务器端防火墙是否允许tcp连接到3306端口



Android与数据库连接

使用mysql 建立连接 是连接的servlet然后给它查询语句，servlet查询后以xml形式返回数据。安卓用HttpClient连接servlet

httpService or jdbc方式



注：连接本地mysql：需要在  服务 中 看打开服务了吗

# 踩坑记录

### 多次编译后，无法运行app，logcat一直出字：可能编译错乱，重新编译Build-> clean poject rebuild 
## 工程各文件夹作用

### 引用的图片，格式放在drawable下



## NDK does not contain any platforms

在ndk-bundle （安装文件下）新建立了一个空文件夹platforms

##  Resources$NotFoundException: String resource ID #0x2fc

 TextView的setText(String str)中，传入的参数是int型的，int参数后加一个 +""，转化为string型即可

## RadioGroup 要想设置默认选中，要给group里的radiobutton设置id，否则获取不到具体的id，只是radiogroup的相对id