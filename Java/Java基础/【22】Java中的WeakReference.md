[TOC]

## 类分层结构

java.lang.ref  

类 WeakReference&lt;T>

```java
- java.lang.Object
    - java.lang.ref.Reference<T>
		- java.lang.ref.PhantomReference<T>
		- java.lang.ref.SoftReference<T>
		- java.lang.ref.WeakReference<T>
    - java.lang.ref.ReferenceQueue<T>
```

## **强引用（StrongReference）** 

强引用是最普遍被使用的引用。 
通常情况下，我们创建一个对象时，使用的都是强引用，例如：

```java
class Test {
    Test() {
        //在栈中创建变量object，其对堆中创建出的对象，就是强引用
        Object object = new Object();
        ...........
    }
}
```

如果一个对象具有强引用，那垃圾回收器绝不会回收它。 当内存空间不足时，**JVM宁愿抛出OOM错误使程序异常**终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题。如果想中断强引用和某个对象之间的关联，可以显示地将引用赋值为null， 这样一来的话，JVM在合适的时间就会回收该对象。

## 弱引用定义

弱引用对象，它们并不禁止其指示对象变得可终结，并被终结，然后被回收。弱引用最常用于实现规范化的映射。  

假定垃圾回收器确定在某一时间点上某个对象是弱可到达对象。这时，它将自动清除针对此对象的所有弱引用，以及通过强引用链和软引用，可以从其到达该对象的针对任何其他弱可到达对象的所有弱引用。同时它将声明所有以前的弱可到达对象为可终结的。在同一时间或晚些时候，它将那些已经向引用队列注册的新清除的弱引用加入队列。

弱引用，与强引用（我们常见的引用方式）相对；特点是：GC在回收时会忽略掉弱引用对象（忽略掉这种引用关系），即：就算弱引用指向了某个对象，但只要该对象没有被强引用指向，该对象也会被GC检查时回收掉。 

什么是内存泄漏？Java使用有向图机制，通过GC自动检查内存中的对象；如果GC发现一个或一组对象为不可达的状态，则将该对象从内存中回收。也就是说：一个对象不被任何引用所指向，则该对象会在被GC发现的时候回收。 



## 一个小例子

```java
A a = new A();

B b = new B(a);
```

当 a=null ，A对象的引用a置空了，a不再指向对象A的地址，我们都知道当一个对象不再被其他对象引用的时候，是会被GC回收的，很显然及时a=null，那么A对象也是不可能被回收的，因为B依然依赖与A，在这个时候，造成了内存泄漏！

```java
A a = new A();

WeakReference wr = new WeakReference(a);

B b = new B((A)wr.get());
```

当 a=null ，这个时候A只被弱引用依赖，那么GC会立刻回收A这个对象，这就是弱引用的好处！他可以在你对对象结构和拓扑不是很清晰的情况下，帮助你合理的释放对象，造成不必要的内存泄漏！！



## 使用Handle出现内存泄漏

**原始方法**

```java
public class MainActivity extends AppCompatActivity {
   private TextView mTextView;
    private Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
           mTextView.setText("");
        }
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loadData();
    }
    private void loadData(){    
        Message message = Message.obtain();
        mHandler.sendMessage(message);
    }

}	
```

当使用内部类（或者匿名类）来创建Handler的时候，**Handler对象会隐式地持有一个外部类的对象**（通常是Activity）的引用（否则怎么可能通过Handler来操作Activity的View？）。而Handler通常会伴随着一个耗时的后台线程（比如：拉取网络图片）；该后台线程在任务执行完毕后，通过消息机制通知Handler，然后Handler把图片更新到界面上。假设用户在网络请求过程中关闭了Activity，正常情况下这个Activity不再被使用，就有可能被GC回收；**但此时线程尚未执行完毕，而该线程持有Handler的引用（不然怎么发送消息给Handler？），Handler又持有Activity的引用，就导致该Activity无法被回收（内存泄漏），直到网络请求结束（如：图片下载完毕）。**另外如果执行了Handler的postDelayed()，该方法会将Handler装入一个Message，并把该Message推到MessageQueue中，由此产生了一条链式结构：MessageQueue->Message->Handler->Activity，导致Activity被持有引用而无法被回收。（**总结：实例对象被其他对象持有引用，而无法被回收**） 。

**使用弱引用后**

```java
public class MainActivity extends AppCompatActivity {
    private MyHandler mHandler = new MyHandler(this);
    private TextView mTextView ;
    private static class MyHandler extends Handler {

        private WeakReference<Context> reference;   //

        public MyHandler(Context context) {
            reference = new WeakReference<>(context);//这里传入activity的上下文
        }
        @Override
        public void handleMessage(Message msg) {
            MainActivity activity = (MainActivity) reference.get();//使用
            if(activity != null){
                activity.mTextView.setText("");
            }
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mTextView = (TextView)findViewById(R.id.textview);
        loadData();
    }

    private void loadData() {
        Message message = Message.obtain();
        mHandler.sendMessage(message);
    }
     @Override
    protected void onDestroy() {
        super.onDestroy();
        mHandler.removeCallbacksAndMessages(null);
    }
}
```

**使用了上述代码后，用户在关闭Activity之后，就算后台线程还没有结束，但由于仅有一个来自Handler的弱引用指向Activity，所有GC仍然会在检查的时候把Activity回收掉。** 

由于Handler持有的对象是使用弱引用，根据WeakReference弱引用的特点在GC回收时能回收弱引用，这样就避免了OOM，另外还有在消息队列中可能会有待处理的消息Message,所以我们可以在onDestroy()或者onStop()中调用mHandler.removeCallbacksAndMessages(null);来移除所有消息和Runnable 



## 其他引用

| 引用类型 | GC回收时间 | 用途           | 生存时间       |
| -------- | ---------- | -------------- | -------------- |
| 强引用   | never      | 对象的一般状态 | JVM停止运行时  |
| 软引用   | 内存不足时 | 对象缓存       | 内存不足时终止 |
| 弱引用   | GC时       | 对象缓存       | GC后终止       |
| 虚引用   | -          | -              | -              |