[TOC]



Javadoc是Sun公司提供的一个技术，它从程序[源代码](https://baike.baidu.com/item/%E6%BA%90%E4%BB%A3%E7%A0%81/3969)中抽取类、方法、成员等注释形成一个和源代码配套的API帮助文档。也就是说，只要在编写程序时以一套特定的标签作注释，在程序编写完成后，通过Javadoc就可以同时形成程序的开发文档了。                                                                                                              --百度百科

文档注释包括三部分：简述、详述、特殊说明部分。

###  各种标记

| 标签                   | 说明                                                         | JDK 1.1 doclet | 标准doclet | 标签类型                            |
| ---------------------- | ------------------------------------------------------------ | -------------- | ---------- | ----------------------------------- |
| @author 作者           | 作者标识                                                     | √              | √          | 包、 类、接口                       |
| @version 版本号        | 版本号                                                       | √              | √          | 包、 类、接口                       |
| @param 参数名 描述     | 方法的入参名及描述信息，如入参有特别要求，可在此注释。       | √              | √          | 构造函数、 方法                     |
| @return 描述           | 对函数返回值的注释                                           | √              | √          | 方法                                |
| @deprecated 过期文本   | 标识随着程序版本的提升，当前API已经过期，仅为了保证兼容性依然存在，以此告之开发者不应再用这个API。 | √              | √          | 包、类、接口、值域、构造函数、 方法 |
| @throws异常类名        | 构造函数或方法所会抛出的异常。                               |                | √          | 构造函数、 方法                     |
| @exception 异常类名    | 同@throws。                                                  | √              | √          | 构造函数、 方法                     |
| @see 引用              | 查看相关内容，如类、方法、变量等。                           | √              | √          | 包、类、接口、值域、构造函数、 方法 |
| @since 描述文本        | API在什么程序的什么版本后开发支持。                          | √              | √          | 包、类、接口、值域、构造函数、 方法 |
| {@link包.类#成员 标签} | 链接到某个特定的成员对应的文档中。                           |                | √          | 包、类、接口、值域、构造函数、 方法 |
| {@value}               | 当对常量进行注释时，如果想将其值包含在文档中，则通过该标签来引用常量的值。 |                | √(JDK1.4)  | 静态值域                            |

![JavaDoc标记](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/urecjEJUJbCO*LKNaDfYPnWg.6syGldrVENCRNhaQm4!/r/dDcBAAAAAAAA)

**常用标签实例讲解**



### 【类】@author、@version 

这两个标记分别用于指明**类**的作者和版本。缺省情况下 javadoc 将其忽略，但命令行开关 -author 和 -version 可以修改这个功能，使其包含的信息被输出。这两个标记的句法如下：  　　

@author 作者名  　　

@version 版本号  　　

其中，@author 可以多次使用，以指明多个作者，生成的文档中每个作者之间使用逗号 (,) 隔开。@version 也可以使用多次，只有第一次有效，生成的文档中只会显示第一次使用 @version 指明的版本号。如下例 

```java
/** 
* @author Fancy 
* @author Bird 
* @version Version 1.00 
* @version Version 2.00 
*/ 
public class TestJavaDoc { 
}
```



### 【方法】 @param、@return 和 @exception

这三个标记都是只用于**方法**的。@param 描述方法的参数，@return 描述方法的返回值，@exception 描述方法可能抛出的异常。它们的句法如下：         

@param 参数名 参数说明         

@return 返回值说明         

@exception 异常类名 说明 

 每一个 @param 只能描述方法的**一个**参数，所以，如果方法需要多个参数，就需要**多次使用 @param** 来描述。  一个方法中**只能用一个 @return**，如果文档说明中列了多个 @return，则 javadoc 编译时会发出警告，且只有第一个 @return 在生成的文档中有效。方法可能抛出的异常应当用 @exception 描述。由于一个方法可能抛出多个异常，所以可以有**多个 @exception**。每个 @exception 后面应有简述的异常类名，说明中应指出抛出异常的原因。

```java
public class TestJavaDoc { 
/** 
* @param n a switch 
* @param b excrescent parameter 
* @return true or false 
* @return excrescent return 
* @exception java.lang.Exception throw when switch is 1 
* @exception NullPointerException throw when parameter n is null 
*/ 
public boolean fun(Integer n) throws Exception { 
       switch (n.intValue()) { 
       case 0: 
              break; 
       case 1: 
              throw new Exception(“Test Only”); 
       default: 
              return false; 
        } 
       return true; 
} 
```

**并没有参数b，也不需要有两个return，这些东西在IDE中会智能提示**

![param等](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/8vZMJfNgcfedQKpmpr8WONQcEAZ6L9HvD6LcmDrAo.Y!/r/dDYBAAAAAAAA)

### @see 的使用

@see 的句法有三种：         

1. @see 类名         
2. @see #方法名或属性名        
3. @see 类名#方法名或属性名 

类名，可以根据需要只写出类名 (如 String) 或者写出类全名 (如 java.lang.String)。那么什么时候只需要写出类名，什么时候需要写出类全名呢？

​       如果 java 源文件中的 import 语句包含了的类，可以只写出类名，如果没有包含，则需要写出类全名。java.lang 也已经默认被包含了。这和 javac 编译 java 源文件时的规定一样，所以可以简单的用 javac 编译来判断，源程序中 javac 能找到的类，javadoc 也一定能找到；javac 找不到的类，javadoc 也找不到，这就需要使用类全名了。

​       方法名或者属性名，如果是属性名，则只需要写出属性名即可；如果是方法名，则需要写出方法名以及参数类型，没有参数的方法，需要写出一对括号。如

![see](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/o26vRbcahGrn7V2iWvP89hXl6HZizkFUL50Sr*.SvaI!/r/dDEBAAAAAAAA)

有时也可以偷懒：假如上例中，没有 count 这一属性，那么参考方法 count() 就可以简写成 @see count。不过，为了安全起见，还是写全 @see count() 比较好。         @see 的第二个句法和第三个句法都是转向方法或者属性的参考，它们有什么区别呢？         第二个句法中没有指出类名，则默认为当前类。所以它定义的参考，都转向本类中的属性或者方法。而第三个句法中指出了类名，则还可以转向其它类的属性或者方法。         关于 @see 标记，我们举个例说明。由于 @see 在对类说明、对属性说明、对方法说明时用法都一样，所以这里只以对类说明为例。 

```java
/** 
* @see java.lang.String 
* @see #str 
* @see #str() 
* @see #main(String[]) 
* @see java.lang.Object#toString() 
*/ 
public class TestJavaDoc 
{ 
       private String str; 
       public void str(){ 
       } 
       public static void main(String[] args){ 
       } 
} 
```

 生成的文档的相关部分如下图： 

![see生成效果](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/6vsTXcMGuj2htSZL9sppmFjPUQtceDwjQqQRF6UQO0s!/r/dFIBAAAAAAAA)



![JavaDoc2](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/djaI74koMeKdzc6KndeZ*4HhtvegGXvljVbS6qIsR6c!/r/dFIBAAAAAAAA)

### 链接

- [一篇很详细的文章](https://blog.csdn.net/GarfieldEr007/article/details/54959597)       
- [一个博客园链接](https://www.cnblogs.com/felix-/p/4310229.html)



\********************************

**以下这一火车的东西暂且用不到**

javadoc [选项][ [软件包](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=48991&ss_c=ssc.citiao.link)名称] [源文件][@file] 
​        -overview <文件> 读取 HTML 文件的概述文档 
　　-public 仅显示公共类和成员 
　　- [protected](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=7667016&ss_c=ssc.citiao.link) 显示受保护/公共类和成员（默认） 
　　-package 显示软件包/受保护/公共类和成员 
　　-private 显示所有类和成员 
　　-help 显示 命令行选项并退出 
　　-doclet <类> 通过替代 doclet 生成输出 
　　-docletpath <路径> 指定查找 doclet 类文件的位置 
　　-sourcepath <路径列表> 指定查找源文件的位置 
　　-[classpath](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=8065780&ss_c=ssc.citiao.link) <路径列表> 指定查找用户类文件的位置 
　　-exclude <软件包列表> 指定要排除的软件包的列表 
　　-subpackages <子软件包列表> 指定要 递归装入的子软件包 
　　-breakiterator 使用 BreakIterator 计算第 1 句 
　　-bootclasspath <路径列表> 覆盖引导类加载器所装入的 
　　类文件的位置 
　　-source <版本> 提供与指定版本的源兼容性 
　　-extdirs <目录列表> [覆盖安装](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=344603&ss_c=ssc.citiao.link)的扩展目录的位置 
　　-verbose 输出有关 Javadoc 正在执行的操作的消息 
　　-locale <名称> 要使用的语言环境，例如 en_US 或 en_US_WIN 
　　-encoding <名称> 源文件编码名称 
　　- quiet 不显示状态消息 
　　-J<标志> 直接将 <标志> 传递给运行时系统

　　-d <directory> 输出文件的目标目录 
　　-use 创建类和包用法页面 
　　-version 包含 @version 段 
　　-author 包含 @author 段 
　　-docfilessubdirs 递归复制文档文件子目录 
　　-splitindex 将索引分为每个字母对应一个文件 
　　-windowtitle <text> 文档的浏览器窗口标题 
　　-doctitle <html-code> 包含概述页面的标题 
　　-header 包含每个页面的 页眉文本 
　　-footer <html-code> 包含每个页面的页脚文本 
　　-top <html-code> 包含每个页面的顶部文本 
　　-bottom <html-code> 包含每个页面的底部文本 
　　- link 创建指向位于 的 javadoc 输出 
　　-linkoffline <url> <url2> 利用位于 <url2> 的包列表链接至位于 
　　档 
　　-excludedocfilessubdir < name1>:..排除具有给定名称的所有文档文件子目 
　　-group : ..在概述页面中，将指定的 包分组 
　　-[nocomment](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=122293387&ss_c=ssc.citiao.link) 不生成描述和标记，只生成声明。 
　　-nodeprecated 不包含 @deprecated 信息 
　　-noqualifier <name1>:<name2>:...输出中不包括指定限定符的列表。 
　　-nosince 不包含 @since 信息 
　　-notimestamp 不包含隐藏 [时间戳](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=7860201&ss_c=ssc.citiao.link) 
　　-nodeprecatedlist 不生成已过时的列表 
　　-notree 不生成类分层结构 
　　-noindex 不生成索引 
　　-nohelp 不生成帮助链接 
　　-nonavbar 不生成 [导航栏](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=415213&ss_c=ssc.citiao.link) 
　　-serialwarn 生成有关 @serial 标记的警告 
　　- tag : :指定单个参数自定义标记
　　-taglet 要注册的 Taglet 的全限定名称 
　　-tagletpath Taglet 的路径 
　　-charset 用于跨平台查看生成的文档的 字符集。 
　　-helpfile <file> 包含帮助链接所链接到的文件 
　　-linksource 以 HTML 格式生成源文件 
　　-sourcetab 指定源中每个 制表符占据的空格数 
　　- keywords 使包、类和成员信息附带 HTML 元标记 
　　-stylesheetfile <path> 用于更改生成文档的样式的文件 
　　-docencoding <name> 输出编码名称 

\************************************