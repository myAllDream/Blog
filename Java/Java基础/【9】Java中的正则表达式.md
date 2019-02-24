### 概念

正则表达式又称规则表达式**。**（英语：Regular Expression，在代码中常简写为regex、regexp或RE），计算机科学的一个概念。正则表达式通常被用来检索、替换那些符合某个模式(规则)的文本。

许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在Perl中就内建了一个功能强大的正则表达式引擎。正则表达式这个概念最初是由Unix中的工具软件（例如sed和grep）普及开的。正则表达式通常缩写成“regex”，单数有regexp、regex，复数有regexps、regexes、regexen。                          --百度百科

正则表达式并不是Java特有的东西。基本上自己写完的正则自己都看不懂，哈哈。我们用正则就是用来做**匹配验证**。

<!--more-->

先看一下JDK手册中的解释：

`public final class Pattern extends Object implements Serializable`

正则表达式的编译表示形式。  

指定为字符串的正则表达式必须首先被编译为此类的实例。然后，可将得到的模式用于创建 Matcher 对象，依照正则表达式，该对象可以与任意字符序列匹配。执行匹配所涉及的所有状态都驻留在匹配器中，所以多个匹配器可以共享同一模式。   

因此，典型的调用顺序是  

```java
 Pattern p = Pattern.compile("a*b");
 Matcher m = p.matcher("aaaaab");
 boolean b = m.matches();
```

在仅使用一次正则表达式时，可以方便地通过此类定义 matches方法。此方法编译表达式并在单个调用中将输入序列与其匹配。语句  

```java
 boolean b = Pattern.matches("a*b", "aaaaab");
```

此类的实例是不可变的，可供多个并发线程安全使用。Matcher 类的实例用于此目的则不安全。

```java
字符

    x 字符 x。举例：'a'表示字符a
    \ 反斜线字符。
    \n 新行（换行）符 ('\u000A') 
    \r 回车符 ('\u000D')
    
字符类

    [abc] a、b 或 c（简单类） 
    [^abc] 任何字符，除了 a、b 或 c（否定） 
    [a-zA-Z] a到 z 或 A到 Z，两头的字母包括在内（范围） 
    [0-9] 0到9的字符都包括    

预定义字符类

    . 任何字符。我的就是.字符本身，怎么表示呢? .
    \d 数字：[0-9]
    \D 非数字:[^\d]/[^0-9]
    \w 单词字符：[a-zA-Z_0-9]
　　 \W 非字符[^\w]

边界匹配器

    ^ 行的开头 
    $ 行的结尾 
    \b 单词边界， 就是不是单词字符的地方。    

Greedy 数量词 

    X? X，一次或一次也没有
    X* X，零次或多次
    X+ X，一次或多次
    X{n} X，恰好 n 次 
    X{n,} X，至少 n 次 
    X{n,m} X，至少 n 次，但是不超过 m 次 

　运算符 

	　　XY 　　	X后跟 Y 
	　　X|Y 　　X 或 Y 
	　　(X) 　　X，作为捕获组

```



### String类中的三个基本操作使用正则：

　　匹配: matches()

　　切割: public **String[]** split(String regex)

　　替换: replaceAll(String regex, String replacement)



### 一些常用正则

```java
/**
     * 验证全部为数字
     */
    public static final String NUMBER_REGEX = "^[0-9]*$";

    /**
     * 手机号正则
     */
    public static final String MOBILE_REGEX = "[1][3,4,5,7,8][0-9]{9}$";

    /**
     * 邮箱正则
     */
    public static final String EMAIL_REGEX = "^\\w+([-+.]\\w+)*@\\w+([-.]\\w+)*\\.\\w+([-.]\\w+)*$";


    /**
     * 车牌号正则
     */
    public static final String PLATE_NUMBER_REGEX = "^[\u0391-\uFFE5]{1}[a-zA-Z0-9]{6}$";


    /**
     * 身份证号正则（18位）
     */
    public static final String ID_CARD_REGEX = "^[1-9]\\d{5}[1-9]\\d{3}((0\\d)|(1[0-2]))(([0|1|2]\\d)|3[0-1])\\d{3}([0-9Xx])$";


    /**
     * URL正则
     */
    public static final String URL_REGEX = "[a-zA-z]+://[^\\s]*";


    /**
     * IP地址正则
     */
    public static final String IP_REGEX = "((2[0-4]\\d|25[0-5]|[01]?\\d\\d?)\\.){3}(2[0-4]\\d|25[0-5]|[01]?\\d\\d?)";


    /**
     * 银行卡号正则
     */
    public static final String BANK_CARD_REGEX = "^\\d{16,19}$|^\\d{6}[- ]\\d{10,13}$|^\\d{4}[- ]\\d{4}[- ]\\d{4}[- ]\\d{4,7}$";


    /**
     * 邮政编码正则
     */
    public static final String POSTAL_CODE_REGEX = "\\d{6}";

```

