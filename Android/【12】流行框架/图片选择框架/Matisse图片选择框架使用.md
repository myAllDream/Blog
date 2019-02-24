[TOC]

[Github地址](https://github.com/zhihu/Matisse)

## 优点

- 简单容易上手
- 支持日夜间模式，可以自定义主题
- 自定义选中图片的最大数
- JPEG, PNG, GIF 图片类型的显示
- 支持有序选择图片，也即选择图片的时候会有 1, 2, 3, 4… 样式的 CheckBox；
- 支持自定义筛选，完全自定义筛选规则
- 可以定义图片缩略图的缩放比例。
- 支持横竖屏。Matisse 做了状态保存的工作
- 支持不同的图片加载库，目前支持 Glide 和 Picasso

<!--more-->

## 使用方法

### 添加依赖

```groovy
repositories {
    jcenter()
}

dependencies {
    compile 'com.zhihu.android:matisse:0.5.0-alpha4'
    compile 'com.github.bumptech.glide:glide:3.7.0'    //Glide升级到4.x后会出问题，需要自己处理
}
```

### 两个权限

Matiss须要添加两个权限，如果是6.0+的版本，须要申请运行时权限，否则报错

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

### 开始

```java
//1
private static final int REQUEST_CODE_CHOOSE = 23;//定义请求码常量

//2
Matisse  
	.from(MainActivity.this)
	.choose(MimeType.allOf())//照片视频全部显示
	.countable(true)//有序选择图片
	.maxSelectable(9)//最大选择数量为9
	.gridExpectedSize(120)//图片显示表格的大小getResources()
     .getDimensionPixelSize(R.dimen.grid_expected_size)
	.restrictOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT)//图像选择和预览活动所需的方向。
	.thumbnailScale(0.85f)//缩放比例
	.theme(R.style.Matisse_Zhihu)//主题  暗色主题 R.style.Matisse_Dracula
	.imageEngine(new GlideEngine())//加载方式
	.forResult(REQUEST_CODE_CHOOSE);//请求码

//3
 List<Uri> mSelected;
@Override      //接收返回的地址
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_CODE_CHOOSE && resultCode == RESULT_OK) {
        mSelected = Matisse.obtainResult(data);
        Log.d("Matisse", "mSelected: " + mSelected);
    }
}
```

## 如果想添加拍照功能

想要使用拍照功能的话，必须要有一个Fileprovider

- 在Android Manifest当中的Application节点下添加FileProvider

```xml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="com.example.fileprovider"   <!--前面换成自己的包名.fileprovider-->
    android:grantUriPermissions="true"
    android:exported="false">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/filepaths" />
</provider>
```

**前面换成自己的包名.fileprovider**

- 然后在Res文件下创建xml文件夹，然后创建filepaths.xml.

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <external-path
        name="my_images"
        path="Pictures"/>
</paths>
```

- 在启动Matisse当中添加如下代码

  ```java
  .capture(true) //是否提供拍照功能
  .captureStrategy(new CaptureStrategy(true,String authority))//存储到哪里 
  ```

**authority**要用provider里的包名.provider。



## Api

| 方法名                | 描述                             | 参数                                                         |
| --------------------- | -------------------------------- | ------------------------------------------------------------ |
| showSingleMediaType() | 仅仅显示一种媒体类型             | Boolean                                                      |
| from()                | 传递当前的Activity或者fragment   | MainActivity.this                                            |
| choose()              | 显示图片的类型                   | MimeType.of(MimeType.JPEG, MimeType.PNG, MimeType.GIF) 或者MimeType.ofAll() |
| countable()           | 是否有序选择图片                 | Boolean                                                      |
| maxSelectable()       | 最大的选择数量                   | int类型 多少都可以                                           |
| addFilter()           | 添加过滤器                       | new GifSizeFilter(320, 320, 5 *Filter.K* Filter.K)，自己重写过滤器 |
| gridExpectedSize()    | 每个图片方格的大小               | 120dp                                                        |
| restrictOrientation() | 设置图像选择和预览活动所需的方向 | ActivityInfo.SCREEN_ORIENTATION_PORTRAIT或者                 |
| thumnailScale()       | 缩放比例                         | 0.1-1之间，一般0.85f                                         |
| imageEngine()         | 使用图片的加载方式               | new GlideEngine()或者new PicassoEngine()                     |
| theme()               | 主题的设置                       | R.style.Matisse_Zhihu 或者 R.style.Matisse_Dracula           |
| forResult()           | 请求码                           | REQUEST_CODE_CHOOSE                                          |

## 包含的类

| 类名                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| ImageEngine            | 图片加载接口，方便后面根据Glide和Picasso分别实现             |
| GlideEngine            | Glide实现ImageEngine                                         |
| PicassoEngine          | Picasso实现ImageEngine                                       |
| Filter                 | 过滤条件抽象类，我们可以通过集成Filter实现对应的过滤条件来对图片进行筛选，可以添加多个Filter |
| Album                  | 相册Entity                                                   |
| CaptureStrategy        | 拍照相关，媒体处理authority                                  |
| IncapableCause         | 信息处理，toast和dialog                                      |
| Item                   | 选择媒体界面的Item                                           |
| SelectionSpec          | 选择参数类                                                   |
| AlbumLoader            | 相册CursorLoader                                             |
| AlbumMediaLoader       | 图片和视频CursorLoader                                       |
| AlbumMediaCollection   | AlbumMediaLoader回调                                         |
| SelectedItemCollection | 被选中项集合                                                 |
| internal/ui包          | 界面显示的Adapter,自定义视图，Fragment和Activity             |
| internal/utils包       | 工具类                                                       |
| MatisseActivity        | 关键类，执行选择媒体操作的时候展示出来的Activity             |
| Matisse                | 开源库的入口和出口，用来传递Activity和Fragment，创建SelectionSpecBuilder和返回结果 |
| MimeType               | 媒体类型                                                     |
| SelectionSpecBuilder   | Build构造类，用来传递参数                                    |