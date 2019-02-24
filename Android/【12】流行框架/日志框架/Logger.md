---
title: Logger
date: 2018-08-29 15:01:06
categories: "Android流行框架"
tags:
---

[Github地址](https://github.com/orhanobut/logger)

## **官网简介**：

Logger:Simple, pretty and powerful logger for android

**简单三步走**

Download

```
implementation 'com.orhanobut:logger:2.2.0'
```

Initialize

```
Logger.addLogAdapter(new AndroidLogAdapter());
```

And use

```
Logger.d("hello");
```

**然后。。。没用的不扯**

<!--more-->

## **开发的时候**

- build.gradle

```groovy
buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        debug {
            buildConfigField("boolean", "LOG_DEBUG", "true")  //定义变量
        }
    }
```

- LogUtil.java

```java
/**
 * Created by ck on 2018/8/29.
 */
public class LogUtil {
    /**
     * 初始化log工具，在app入口处调用
     *
     * @param isLogEnable 是否打印log
     */
   //参数在Application文件中传入。
    public static void init(final boolean isLogEnable) {

        FormatStrategy formatStrategy = PrettyFormatStrategy.newBuilder()
                .showThreadInfo(false)  // (Optional) Whether to show thread info or not. Default true
                .methodCount(0)         // (Optional) How many method line to show. Default 2
                .methodOffset(7)        // (Optional) Hides internal method calls up to offset. Default 5
               // .logStrategy(customLog) // (Optional) Changes the log strategy to print out. Default LogCat
                .tag("ck")   // (Optional) Global tag for every log. Default PRETTY_LOGGER
                .build();

        Logger.addLogAdapter(new AndroidLogAdapter(formatStrategy) {
            @Override
            public boolean isLoggable(int priority, @Nullable String tag) {
                return isLogEnable;
            }
        });
    }

    public static void d (String message){
        Logger.d(message);
    }

    public static void i (String message){
        Logger.i(message);
    }

    public static void w (String message, Throwable e){
        String info = e != null ? e.toString() : "null";
        Logger.w(message + "：" + info);
    }

    public static void e (String message, Throwable e){
        Logger.e(e, message);
    }

    public static void json (String json){
        Logger.json(json);
    }
}

```

- APP.java

```java
public class App extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        LogUtilPlus.init(BuildConfig.LOG_DEBUG);  //Build.gradle里定义的变量。
}
```

```
Logger.t(TAG).d(message);
```

logger.t() 可用于打印临时Tag,不同于Application 初始化中的TAG 这样就非常方便了