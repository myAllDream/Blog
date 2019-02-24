[TOC]

Java中最常见的错误就是空指针异常**

**NullPointerException由RuntimeException派生出来，是一个运行级别的异常。意思是说可能会在运行的时候才会被抛出，而且需要看这样的运行级别异常是否会导致你的业务逻辑中断。 **

在面向对象的语言中，指针也是对象的引用。而空指针，就是指针的内容为空（也可以理解为这个指针没有指向一块内存）。由于这是一个空的指针，指向了声明类型的类的空对象，所以你在应用这个对象的属性或者方法的时候，自然是错误的，也就是会报空指针异常。



## 例子

```java
public class Student{
    private String name;
    void setName(String name){
        this.name=name;
    }
     void getName(){
        return name;
    }
}
//以下操作会导致空指针异常
Student A;
A.getName("ck");
```

## 以下几种情况会导致空指针异常

- 字符串变量未初始化；  
- 接口类型的对象没有用具体的类初始化，比如：  List it；会报错  List it = new ArrayList()；则不会报错了  
- 当一个对象的值为空时，你没有判断为空的情况。

## 解决办法

- 写程序时严谨些，尽量避免了，例如在拿该变量与一个值比较时，要么先做好该异常的处理如： if (str == null) {   System.out.println("字符为空!"); } 当然也可以将这个值写在前面进行比较的，例如，判断一个String的实例s是否等于“a”，不要写成s.equals("a")，这样写容易抛出NullPointerException，而写成"a".equals(s)就可以避免这个问题。不过对变量先进行判空后再进行操作比较好   。
- 尽量避免返回null，方法的返回值不要定义成为一般的类型，而是用数组。这样如果想要返回null的时候，就返回一个没有元素的数组。就能避免许多不必要的NullPointerException。