

[说明网站](http://jakewharton.github.io/butterknife/)

[github](https://github.com/JakeWharton/butterknife)

<!--more-->

### 简介

**ButterKnife 优势：**

1.强大的View绑定和Click事件处理功能，简化代码，提升开发效率

2.方便的处理Adapter里的ViewHolder绑定问题

3.运行时不会影响APP效率，使用配置方便

4.代码清晰，可读性强

**使用心得：**

1.Activity ButterKnife.bind(this);必须在setContentView();之后，且父类bind绑定后，子类不需要再bind

2.Fragment ButterKnife.bind(this, mRootView);

3.属性布局不能用private or static 修饰，否则会报错

4.setContentView()不能通过注解实现。（其他的有些注解框架可以）

### 使用方法

- gradle中引依赖

```groovy
dependencies {
  implementation 'com.jakewharton:butterknife:8.8.1'
  annotationProcessor 'com.jakewharton:butterknife-compiler:8.8.1'
}
```

- 装Android ButterKnife Zelezny插件
- 直接用就行了