
我是真的不知道干了啥，觉得没动啥呢，无缘无故的 `Error:Please select Android SDK`

<!--more-->

![ Please select Android SDK ](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/iVCiO*rQz6yQCtbgUJoGj3kU15PEJbr3WAC5EMVBZsk!/r/dFQBAAAAAAAA)

百度了一下：

- 打开app.iml文件
- 找一下有没有这样一行

```xml
<orderEntry type="jdk" jdkName="Android API 27 Platform" jdkType="Android SDK" />
```

- 没有则添加。