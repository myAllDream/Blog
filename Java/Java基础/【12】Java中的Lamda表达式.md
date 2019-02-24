使用 Lambda 表达式原因

Lambda 是一个匿名函数，可以把 Lambda表达式 理解为是一段可以传递的代码 (将代码像数据一样进行传递)。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升

Lambda 表达式的基础语法 : Java8 中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符，箭头操作符将 Lambda 表达式拆分成两部分 :

左侧 : Lambda 表达式的参数列表

右侧 : Lambda 表达式中所需执行的功能， 即 Lambda 体

<!--more-->

**Lambda表达式的语法** 

基本语法: 

- **(参数) ->单行语句；**
- **(参数) ->{多行语句}；**
- **(参数) ->表达式；**

- 语法格式一 : 无参数，无返回值

```java
() -> System.out.println("Hello Lambda!");
```

- 语法格式二 : 有一个参数，并且无返回值

```java
(x) -> System.out.println(x)
```

- 语法格式三 : 若只有一个参数，小括号可以省略不写

```java
x -> System.out.println(x)
```

- 语法格式四 : 有两个以上的参数，有返回值，并且 Lambda 体中有多条语句

```java
Comparator**<Integer> com = (x, y) -> {

    System.out.println("函数式接口");

    return Integer.compare(x, y);

};
```

- 语法格式五 : 若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写

```java
Comparator**<Integer> com = (x, y) -> Integer.compare(x, y);
```

- 语法格式六 : Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断”

```java
(Integer x, Integer y) -> Integer.compare(x, y);
```

**注 : Lambda 表达式中的参数类型都是由编译器推断得出的。 Lambda 表达式中无需指定类型，程序依然可以编译，这是因为 javac 根据程序的上下文，在后台推断出了参数的类型。 Lambda 表达式的类型依赖于上下文环境，是由编译器推断出来的。这就是所谓的 “类型推断”**

暂且到这里------
