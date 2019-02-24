[TOC]

本来在用[Hexo生成的静态博客](https://ck76.github.io/)，但是后来发现文件多的话生成很慢，机智的弃了，转战github，文章是学习过程中的笔记与生活中的记录，希望可以帮到你~ PS：我觉得这里是个好地方呦：）

> **推荐使用Chrome+Octotree查看更佳**

```java
##########目录##########
├── Android
├── Gradle
├── Java
├── Kotlin
├── Tips
├── 日语
├── 生活
├── 在编辑blog
├── 学校课程
├── 杂七杂八
├── 胡搞瞎搞
├── 设计模式
├── 读书笔记
├── 辅助工具
├── 问题解决
└── 数据结构与算法
```



# 🍀Android🥑

```java
.
├── 【0】高级
│   ├── App安装流程.md
│   ├── IPC
│   │   ├── Binder.md
│   │   └── 【转】Binder机制.md
│   ├── Surface
│   │   ├── Surface.md
│   │   └── 绘制机制以及Surface源码.md
│   ├── XMS家族
│   │   └── WMS
│   │       ├── Activity中Window的创建过程.md
│   │       └── WMS的add、remove...md
│   ├── 组件系列
│   │   ├── Activity启动流程.md
│   │   └── Service启动流程的.md
│   └── 系统启动流程.md
├── 【10】版本特性
│   ├── Android各个版本简介.md
│   ├── 【6.0】运行时权限管理.md
│   ├── 【7.0】FileProvider解析.md
│   └── 【7.0】拍照裁剪适配问题.md
├── 【11】架构模式
│   ├── MVP.md
│   ├── MVVM.md
│   └── 模块化.md
├── 【12】流行框架
│   ├── 事件总监
│   │   └── EventBus.md
│   ├── 日志框架
│   │   └── Logger.md
│   ├── 数据库框架
│   │   ├── GreenDAO.md
│   │   ├── GreenDaoUtil.md
│   │   └── RxGreenDao.md
│   ├── 依赖注入框架
│   │   └── ButterKnife.md
│   ├── 图片加载框架
│   │   ├── Glide.md
│   │   ├── Glide、Picasso、Fresco对比.md
│   │   └── Glide源码解析.md
│   ├── 图片选择框架
│   │   └── Matisse图片选择框架使用.md
│   ├── 性能优化框架
│   │   └── LeakCanary.md
│   ├── 网络解析框架
│   │   └── Gson.md
│   ├── 网络请求框架
│   │   ├── OkHttp源码解析.md
│   │   ├── Retrofit2.0.md
│   │   ├── Retrofit自定义GsonConverter处理请求错误异常处理.md
│   │   └── Retrofit源码解析.md
│   └── 响应式编程框架
│       ├── RxJava 2.x.md
│       └── RxLifecycle.md
├── 【13】项目开发记录
│   ├── 牛课
│   │   ├── 牛课.md
│   │   └── 烈焰弹幕使.md
│   └── 项目开发中遇到的问题.md
├── 【14】项目阅读笔记
│   ├── Taskgo
│   │   ├── Android菜鸟教程阅读笔记.md
│   │   └── Taskgo研究笔记.md
│   ├── UtilCode
│   │   ├── SharedPreferenceUtil.md
│   │   └── UtilCodeREADME.md
│   ├── sunflower
│   │   ├── GardenFragment.md
│   │   ├── PlantDetailFragment.md
│   │   ├── PlantListFragment.md
│   │   ├── 源代码
│   │   └── 项目简介.md
│   └── 云想
│       ├── account
│       │   └── account.md
│       ├── base
│       │   ├── paging类图.md
│       │   └── paging基础库封装.md
│       ├── config
│       │   └── config.md
│       ├── home
│       ├── lib
│       │   ├── lib-mock.md
│       │   ├── lib-network.md
│       │   ├── lib-permission.md
│       │   ├── lib-runtime.md
│       │   ├── lib-scheme.md
│       │   ├── lib-splash.md
│       │   ├── lib-util.md
│       │   └── lib-webview.md
│       ├── main
│       │   └── main.md
│       ├── network
│       │   └── network.md
│       ├── news
│       │   └── news.md
│       ├── personal
│       │   └── personal.md
│       ├── price
│       │   └── price.md
│       ├── product
│       │   └── product.md
│       ├── search
│       │   ├── search.md
│       │   └── 类图.md
│       └── 总览.md
├── 【15】其他
│   ├── Android手机的各种架构.md
│   └── 混淆
│       ├── Android混淆配置.md
│       └── 混淆.md
├── 【16】JetPack
│   ├── App架构指导.md
│   ├── DataBinding.md
│   ├── JetPack目录.md
│   ├── JetPack的设计.md
│   ├── Lifecycle.md
│   ├── LiveData.md
│   ├── LiveData官方文档.md
│   ├── Navigation.md
│   ├── Paging.md
│   ├── Room.md
│   ├── ViewModel.md
│   └── WorkManager.md
├── 【17】源码阅读
│   ├── BaseRecyclerViewAdapterHelper
│   │   └── 新建文件.md
│   ├── FlowLayout
│   │   ├── FlowLayout.md
│   │   ├── README.md
│   │   ├── TagAdapter.md
│   │   ├── TagFlowLayout.md
│   │   └── TagView.md
│   ├── JetPack
│   │   └── 链接.md
│   ├── RecyclerView
│   │   └── 链接.md
│   └── baseAdapter
│       └── baseAdapter.md
├── 【18】github的awesome-adb
│   ├── README.md
│   ├── assets
│   │   ├── title.png
│   │   └── toc.png
│   └── related
│       ├── aapt.md
│       ├── am.md
│       ├── dumpsys.md
│       ├── pm.md
│       └── uiautomator.md
├── 【1】Android
│   ├── Android Studio项目结构.md
│   ├── Android Studio常用快捷键.md
│   ├── Android知识体系.md
│   ├── Android开源库搜集.md
│   ├── 【转】App Bundle.md
│   └── 【转】Dalvik和ART.md
├── 【2】基础
│   ├── Android-uri.md
│   ├── Android原生API实现分享功能.md
│   ├── Android应用基础知识.md
│   ├── AppCompatActivity解析.md
│   ├── Application类解析.md
│   ├── BuildConfig类解析.md
│   ├── Intent和Bundle.md
│   ├── LayoutInflater类解析.md
│   ├── R-java文件解析.md
│   ├── SpannableString解析.md
│   └── 【转】Context解析.md
├── 【3】五大组件
│   ├── Activity
│   │   ├── Activity解析.md
│   │   └── 常用Intent意图表.md
│   ├── BroadcastReceiver
│   │   └── BroadcastReceiver基础.md
│   ├── ContentProvider
│   │   └── ContentProvider基础.md
│   ├── Fragment
│   │   ├── Fragment.md
│   │   └── Frangment懒加载.md
│   └── Service
│       └── Service.md
├── 【4】持久化数据
│   ├── Android中的存储位置.md
│   ├── SharedPreferences存储数据.md
│   └── serializable和parcelable.md
├── 【5】Android多线程
│   ├── Android多线程概括.md
│   ├── AsyncTask.md
│   └── Handler.md
├── 【6】View
│   ├── Resource
│   │   ├── assets
│   │   │   └── assets解析.md
│   │   └── res
│   │       ├── Drawable
│   │       │   ├── Drawable汇总.md
│   │       │   ├── layer-list解析.md
│   │       │   ├── selector解析.md
│   │       │   └── shape解析.md
│   │       └── values
│   │           └── values解析.md
│   ├── Support Design
│   ├── View的事件体系
│   │   ├── 1.View基础知识.md
│   │   ├── 2.View的滑动.md
│   │   ├── 3.弹性滑动.md
│   │   ├── 4.View的事件分发机制.md
│   │   └── 对getScrollX()的理解.md
│   ├── View的工作原理
│   │   ├── ATMOST解决办法.md
│   │   ├── Android视图架构详解.md
│   │   └── 绘制.md
│   ├── WebView
│   │   └── WebView解析.md
│   ├── recyclerview
│   │   ├── 【10】侧滑菜单.md
│   │   ├── 【11】RecyclerView与ListView比较.md
│   │   ├── 【13】滚动到指定位置.md
│   │   ├── 【14】notifyDataSetChanged()无效.md
│   │   ├── 【15】Listview的重用机制.md
│   │   ├── 【16】ListView和Recyclerview重用机制对比.md
│   │   ├── 【17】ItemDecoration.md
│   │   ├── 【18】DiffUtil.md
│   │   ├── 【19】ItemAnimator.md
│   │   ├── 【1】基本使用.md
│   │   ├── 【20】SnapHelper.md
│   │   ├── 【2】封装BaseAdapter.md
│   │   ├── 【3】OnSrollListener.md
│   │   ├── 【4】Taskgo封装的RecyclerView.md
│   │   ├── 【6】RecyclerView整理.md
│   │   ├── 【7】与Tablayout联动.md
│   │   ├── 【8】二级列表式RecyclerView.md
│   │   ├── 【9】拖动排序.md
│   │   └── 目录.md
│   ├── 动画
│   │   └── 1.动画基本使用方法.md
│   └── 自定义view
│       ├── 【1】API总览.md
│       ├── 【2】基础.md
│       ├── 【3】API速查.md
│       ├── 链接.md
│       └── 待练清单.md
├── 【7】图像相关
│   ├── 【1】Bitmap(位图).md
│   └── 【2】Bitmao占用内存大小.md
├── 【8】音视频
│   └── Untitled.md
└── 【9】性能优化
    ├── UI优化
    │   └── Android屏幕适配.md
    ├── 内存优化
    │   ├── ANR分析.md
    │   ├── ArrayMap.md
    │   ├── 主线程Looper.loop()死循环为什么不会造成ANR.md
    │   ├── 内存抖动.md
    │   └── 内存泄漏.md
    └── 启动优化.md
```



---

# ☕️Java

```java
.
├── JVM合集
│   ├── JVM【0】之常见面试题.md
│   ├── JVM【1】之内存结构.md
│   ├── JVM【2】之垃圾回收.md
│   ├── JVM【3】之类装载机制.md
│   ├── JVM【4】之class文件结构.md
│   └── 类加载.md
├── Java基础
│   ├── 【11】Java泛型.md
│   ├── 【12】Java中的Lamda表达式.md
│   ├── 【13】Java中的接口回调.md
│   ├── 【14】Java中的反射.md
│   ├── 【15】Java中的注解-Annotation.md
│   ├── 【16】Java中的空指针异常.md
│   ├── 【17】Java中的内存溢出和内存泄漏.md
│   ├── 【19】Java中的代理模式.md
│   ├── 【20】Java中的移位操作符.md
│   ├── 【21】生产者消费者.md
│   ├── 【22】Java中的WeakReference.md
│   ├── 【2】JavaDoc.md
│   ├── 【3】Java面向对象三大特征.md
│   ├── 【4】Java的基本数据类型及其包装类.md
│   ├── 【5】Java中的变量类型.md
│   ├── 【6】Java中的常用类.md
│   ├── 【7】Java中的异常体系.md
│   ├── 【8】Java中的内部类.md
│   ├── 【9】Java中的正则表达式.md
│   └── 【~】JDBC.md
├── Java集合
│   ├── ConcurrentHashMap(JDK1.7).md
│   ├── HashMap基础.md
│   ├── Java集合类.md
│   └── 高并发下的HashMap (JDK1.7).md
└── Java多线程
    ├── Java中的各种锁.md
    ├── Java中的多线程关键字.md
    ├── Synchronized.md
    ├── ThreadLocal.md
    └── 目录.md
```

---

# ☕️Kotlin

```java
.
├── Kotlin特性
│   ├── 单例.md
│   ├── 常量.md
│   ├── 【转】Java和Kotlin中的协变、逆变和不变.md
│   ├── 【转】companion object与object区别.md
│   ├── 【转】用Kotlin提高生产力.md
│   └── 【转】不要用Java的语法思想写Kotlin.md
├── kotlin基础
│   ├── 入门.md
│   ├── 作用域函数.md
│   ├── 常用操作符.md
│   └── 集合操作符.md
├── kotlin官方文档翻译
│   ├── Basics
│   │   ├── 包.md
│   │   ├── 基本类型.md
│   │   ├── 流程控制.md
│   │   └── 返回与跳转.md
│   ├── ClassesAndObjects
│   │   ├── 扩展.md
│   │   ├── 接口.md
│   │   ├── 泛型.md
│   │   ├── 嵌套类.md
│   │   ├── 数据类.md
│   │   ├── 枚举类.md
│   │   ├── 类代理.md
│   │   ├── 代理属性.md
│   │   ├── 类和继承.md
│   │   ├── 属性和字段.md
│   │   ├── 可见性修饰词.md
│   │   └── 对象表达式和声明.md
│   ├── FAQ
│   │   ├── FAQ
│   │   └── 与 java 的对比.md
│   ├── FunctionsAndLambdas
│   │   ├── 函数.md
│   │   ├── 协程.md
│   │   ├── 内联函数.md
│   │   └── 高阶函数与 lambda 表达式.md
│   ├── GettingStarted
│   │   ├── Kotlin 1.1 新特性.md
│   │   ├── 习语.md
│   │   ├── 基本语法.md
│   │   └── 编码规范.md
│   ├── Other
│   │   ├── Ranges.md
│   │   ├── This 表达式.md
│   │   ├── 反射.md
│   │   ├── 异常.md
│   │   ├── 注解.md
│   │   ├── 相等.md
│   │   ├── 空安全.md
│   │   ├── 动态类型.md
│   │   ├── 多重声明.md
│   │   └── 类型检查和转换.md
│   ├── README.md
│   ├── SUMMARY.md
│   └── Tools
│       ├── Documenting-Kotlin-Code.md
│       ├── README.md
│       ├── Using-Ant.md
│       ├── Using-Griffon.md
│       ├── Using-Maven.md
│       └── 使用 Gradle.md
├── 协程
│   ├── 【0】什么是协程.md
│   ├── 【1】协程基础.md
│   └── 【2】协程的挂起、恢复和调度.md
├── 文章链接.md
└── 极客时间学习笔记
    ├── 【1】基础语法.md
    ├── 【2】与Java完全兼容.md
    ├── 【3】新手使用Kotlin常遇到的问题.md
    ├── 【4】函数与lambda闭包.md
    ├── 【5】类与对象.md
    ├── 【6】高级特性.md
    └── 【7】协程.md
```



----

# 🔋 数据结构与算法

```java
.
├── 算法
├── 剑指Offer(Java)
│   ├── leet待实现
│	├── 【1】数据结构
│	├── 【2】算法和数据操作
│	├── 【3】高质量代码
│	├── 【4】解决面试题的思路
│	├── 【5】优化时间和空间效率
│	├── 【6】面试中的各项能力
│	├── 【7】案例
│	└── 【8】LeetCode
└── 数据结构
```



---

# 💻学校课程

```java
.
├── 操作系统
│   ├── 内存映射.md
│   └── 必会知识.md
├── 数据库原理
│   └── 主键、外键、索引.md
├── 计算机网络
│   ├── HTTP、TCP:IP.md
│   ├── Http与Https.md
│   └── 一些地址.md
├── 计算机组成原理
│   └── NULL.md
└── 汇编语言程序设计
    └── 链接.md
```



---

# 💤 设计模式

```java
.
├── 创建型模式
│   └── 单例模式.md
├── 结构型模式
│   └── 代理模式.md
├── 行为型模式
│   ├── 观察者模式.md
│   └── 责任链模式.md
├── 源码设计模式.md
└── 设计模式概括.md
```



---

# 🐕 辅助工具

```java
.
├── Git
│   ├── Git.md
│   ├── Git冲突产生的原因.md
│   ├── git-commit-amend.md
│   ├── git-config.md
│   ├── git-log.md
│   ├── git-rebase vs git-merge.md
│   ├── git-stash.md
│   ├── git-tag.md
│   ├── git常用命令.md
│   └── 提交审查.md
├── IDEA快捷键.md
├── MAC里的好软件.md
├── homebrew常用命令.md
├── markdown
│   └── markdown表情.md
├── tree常用命令.md
└── 实用网站.md
```



---

# 🐟 杂七杂八

```java
.
├── Bookmark 下午5.51.08 下午5.51.08.html
├── UML类图.md
├── 假期计划.md
├── 开源许可证.md
├── 系列文章链接.md
└── 【转】收藏的网站.md
```



---

# 🎵胡搞瞎搞

```java
.
├── Hexo-GitHub搭建个人博客及个性化配置.md
├── Hexo使用中的问题-持续补充.md
└── 解决Intellij占用C盘过大的问题.md
```



---

# 💰 面试

```java
.
├── HR问题.md
├── 【10】JetPack面试题.md
├── 【1】数据结构与算法面试题.md
├── 【2】计算机网络面试题.md
├── 【3】Java基础面试题.md
├── 【4】Java多线程面试题.md
├── 【5】Java集合类面试题.md
├── 【6】JVM面试题.md
├── 【7】Kotlin面试题.md
├── 【8】Android基础面试题.md
├── 【9】Android开源库.md
├── 一些文章.md
├── 热点目录.md
└── 项目相关.md
```



---

# 🐸 生活

```java
.
├── 电影
│   ├── 一些剧.md
│   ├── 纪录片.md
│   ├── 印度电影.md
│   ├── 国语电影.md
│   ├── 我和电影.md
│   ├── 日本电影.md
│   ├── 法国电影.md
│   ├── 英美电影.md
│   ├── 韩国电影.md
│   └── 其他国家电影.md
├── 日本自由行规划
│   ├── App.md
│   ├── 交通.md
│   ├── 住宿.md
│   ├── 日程.md
│   ├── 景点.md
│   ├── 美食.md
│   ├── 花费.md
│   ├── 资源.md
│   ├── 前期准备.md
│   └── 行程安排.md
└── 程序员养生：）
    └── 减脂知识小总结.md
```



---

# :cloud: 日语

```java
.
├── 形容词和动词变形总结.md
├── 敬体形和简体形变形总结.md
└── 东北大学秦皇岛分校日本大学交流项目介绍.md
```



