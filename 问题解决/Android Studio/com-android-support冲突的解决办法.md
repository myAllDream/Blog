**All com.android.support libraries must use the exact same version specification (mixing versions can lead to runtime crashes**

当我们使用android studio添加一些第三方的依赖库时，很可能会提示上面这个错误。

大致意思就是com.android.support的包版本号要保持一致，但是可能我们自己新建的项目的com.android.support包版本号要高一些，一些第三方的库的com.android.support可能没有及时更新support库，就会出现这个错误。

解决方法（同样的适用于其他的依赖冲突。） 
1）修改自己项目中的com.android.support的版本号，与所依赖的库版本号一致，但是当我们依赖的库中的com.android.support版本号有好几个版本就不行了。**（不推荐）**

2）推荐这种方法，如果发生冲突了，依赖第三方库时候排除掉对com.android.support包的依赖，这样自己的项目随便依赖什么版本都可以。

**exclude group:表示只要包含com.android.support的都排除** 
api是android studio3.0中新的依赖方式

例如：

```gradle
    api("com.afollestad.material-dialogs:core:0.9.5.0") {
        exclude group: 'com.android.support'
    }
```

**module：删排除group中的指定module** 
例如：

```gradle
    api("com.afollestad.material-dialogs:core:0.9.5.0") {
        exclude group: 'com.android.support', module: 'support-v13'
        exclude group: 'com.android.support', module: 'support-vector-drawable'
    }
```

另外还有一个建议，在我们自己创建library给别人使用时，如果需要依赖com.android.support的话，**建议用provided的方式依赖（android studio3.0中更改为compileOnly）**，这样只会在编译时有效，不会参与打包。以免给使用者带来不便。

例：

```gradle
    provided 'com.android.support:appcompat-v7:26.1.0'
    provided 'com.android.support:design:26.1.0'
    provided 'com.android.support:support-vector-drawable:26.1.0'
```
