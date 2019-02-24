[TOC]

 在应用开发中，接口回调机制是一种常用的设计手段，也可以说是一种处理问题的模型，类之间，模块之间，都有一定的调用关系，一般来说，可以把使用某一接口的类创建的对象的引用赋给该接口声明的接口变量，那么该接口变量就可以调用被类实现的接口方法，实际上，当接口变量调用被类实现的接口中的方法时，就是通知相应的对象调用接口的方法，这个调用的过程称为接口的回调，是不是很晕乎，无奈，理论总是很抽象，后面我们会通过具体的实例来验证，看完实例大家可以再回来体会一下，看看是不是这个理 
既然接口回调是开发应用中处理问题的核心手段，那我们来看看它到底有什么应用场景，也就是说具体在什么情况下，我们要想到使用接口回调来解决问题，回调一般用于分层间的互相协作，上层将本层函数安装在下层，这个函数就是回调，而下层在一定条件下触发回调，例如，作为一个驱动，是底层，它在收到一个数据时，除了完成本层的处理工作外，还将进行回调，将数据交给上层做进一步的处理，另外我们在封装中也经常用到，还有就是 View 的点击事件其实也是使用回调的原理 
原理：首先创建一个对象，然后再创建一个控制器对象，将回调对象需要被调用的方法告诉控制器对象，控制器对象负责检查某个场景是否出现或某个条件是否满足，当满足时，自动调用回调对象方法

# 一种

## 先定义一个接口

```java
//定义一个接口
3 public interface JieKou {
4 	public void show();
5 }
```

## 定义一个Boss类实现接口


```java
public class Boss implements JieKou{
 4 //定义一个老板实现接口
 5     @Override
 6     public void show() {
 7         System.out.println("知道了");
 8     }
 9 
10 }
```

## 定义一个员工Employee类


```java
public class Employee {
 4 //接口属性，方便后边注册
 5     JieKou jiekou;
 6 //注册一个接口属性，等需要调用的时候传入一个接口类型的参数，即本例中的Boss和Employee，
 7 public Employee zhuce(JieKou jiekou,Employee e){
 8     this.jiekou=jiekou;
 9     return e;
10 }
11 public void dosomething(){
12     System.out.println("拼命做事，做完告诉老板");
13     //接口回调，如果没有注册调用，接口中的抽象方法也不会影响dosomething
14     jiekou.show();
15 }
16 
17 }
```

## 测试类


```java
public class Test {
public static void main(String[] args) {
    Employee e=new Employee();
    //需要调用的时候先注册,传入Boss类型对象和员工参数
    Employee e1=e.zhuce(new Boss(),e);
    e1.dosomething();
}
}
```


# 另一种

## 一般用法

新建入口类Main，并新建接口InterfaceExample

```java
public class Main implements InterfaceExample{

    public static void main(String[] args) {
        System.out.println("------接口使用测试--------");
        InterfaceTest test = new InterfaceTest();
        //调用InterfaceTest的handleThings方法，并传递Main的实例
        System.out.println("------异步回调测试--------");
        test.handleThings(new Main());
    }

    @Override   //重写接口方法
    public void sendMessage(String string) {
        System.out.println("接口回调成功，利用 " + string + " 做一些事");
    }
}

//接口也可以写在一个独立的.java文件里
interface InterfaceExample {
    void sendMessage(String string);
}
```

下面新建发起回调的类InterfaceTest

```java
public class InterfaceTest {

    //注意这里Main实例向上转型，接口变量引用了Main实例
    public void handleThings(InterfaceExample example) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("-----做一些事------");
                try {
                    Thread.sleep(3000);         
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                //回调接口方法
                example.sendMessage("接口传的参数");
            }
        }).start();
    }
}
```

最后运行输出：

```java
------接口使用测试--------
------异步回调测试--------
-----做一些事------
接口回调成功，利用 接口传的参数 做一些事
```

其中异步的处理也可以在Main中调用handleThings时进行。



## 结合匿名内部类实现接口回调

第二种方法只需要在第一种的基础上修改Main类就可以

```java
public class Main {

    public static void main(String[] args) {
        System.out.println("------接口使用测试--------");     
        InterfaceTest test = new InterfaceTest();
        //调用InterfaceTest的handleThings方法，并使用实现了InterfaceExample接口的匿名内部类
        //在该匿名内部类中重写接口方法
        test.handleThings(new InterfaceExample() {
            @Override    //重写接口方法
            public void sendMessage(String string) {
                System.out.println("接口回调成功，利用 " + string + " 做一些事");
            }
        });
        System.out.println("------异步回调测试--------");
    }
}

interface InterfaceExample {
    void sendMessage(String string);
}
```

可以看到，采用匿名内部类的方式可以简化代码，使程序结构更清晰。所以这种用法很常见，比如android系统提供的view的点击事件就是采用这种形式进行回调。 
输出是一样的：

```java
------接口使用测试--------
------异步回调测试--------
-----做一些事------
接口回调成功，利用 接口传的参数 做一些事
```

#  总结

- 定义接口
- 被回调的对象实现接口并重写方法
- 执行回调的类要有一个方法(形参是接口)，接收实现接口的类实例(类转接口)，进行回调。



# Android

.View 的回调接口 

```java
/** 
 * Interface definition for a callback to be invoked when a view is clicked. 
 */  
public interface OnClickListener {  
    /** 
     * Called when a view has been clicked. 
     * 
     * @param v The view that was clicked. 
     */  
    void onClick(View v);  
}  
```

回调函数 

```java
/** 
 * Callback 测试 
 */  
public class MainActivity extends AppCompatActivity implements View.OnClickListener{  
  
    private Button button;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
        initView();  
        init();  
    }  
  
    private void initView() {  
        button  = (Button) findViewById(R.id.button);  
        button.setOnClickListener(this);  
    }  
  
    private void init() {  
  
        ProgrammerB programmerB = new ProgrammerB();  
  
        ProgrammerA programmerA = new ProgrammerA(programmerB);  
  
        programmerA.doEvent("编写一个列表界面");  
    }  
  
    /** 
     * 用户点击Button时调用的回调函数 
     * @param v 
     */  
    @Override  
    public void onClick(View v) {  
        Toast.makeText(this,"onClick",Toast.LENGTH_SHORT).show();  
    }  
}  
```



.View 类的 setOnClickListener 方法 

```java
public class View implements Drawable.Callback, KeyEvent.Callback, AccessibilityEventSource {    
    /**  
     * Listener used to dispatch click events.  
     * This field should be made private, so it is hidden from the SDK.  
     * {@hide}  
     */    
    protected OnClickListener mOnClickListener;    
        
    /**  
     * setOnClickListener()的参数是OnClickListener接口 
     * Register a callback to be invoked when this view is clicked. If this view is not  
     * clickable, it becomes clickable.  
     *  
     * @param l The callback that will run  
     *  
     * @see #setClickable(boolean)  
     */    
        
    public void setOnClickListener(OnClickListener l) {    
        if (!isClickable()) {    
            setClickable(true);    
        }    
        mOnClickListener = l;    
    }    
        
        
    /**  
     * Call this view's OnClickListener, if it is defined.  
     *  
     * @return True there was an assigned OnClickListener that was called, false  
     *         otherwise is returned.  
     */    
    public boolean performClick() {    
        sendAccessibilityEvent(AccessibilityEvent.TYPE_VIEW_CLICKED);    
    
        if (mOnClickListener != null) {    
            playSoundEffect(SoundEffectConstants.CLICK);    
                
            //回调方法   
            mOnClickListener.onClick(this);    
            return true;    
        }    
    
        return false;    
    }    
}   
```

# Retrofit里

使用 Retrofit 的步骤共有7个：

**步骤1：**添加Retrofit库的依赖 
**步骤2：**创建 接收服务器返回数据 的类 
**步骤3：**创建 用于描述网络请求 的接口 
**步骤4：**创建 Retrofit 实例 
**步骤5：**创建 网络请求接口实例 并 配置网络请求参数 
**步骤6：**发送网络请求（异步 / 同步）

网络请求操作类：

```java
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("http://taskgobe.sealbaby.cn/")
                .addConverterFactory(GsonConverterFactory.create())
                .build();
       //通过反射创建网络接口实例
         RetrofitPost retrofitPost = retrofit.create(RetrofitPost.class);
        //Body？？？？？？？？？？？？？？
        RequestBody requestBody=RequestBody.create(MediaType.parse("application/json; charset=utf-8"),ck);
        retrofit2.Call<com.ck.myapplication.Response> call = retrofitPost.getCall(requestBody);
        //发送网络请求
        call.enqueue(new Callback<com.ck.myapplication.Response>() {
            @Override
            public void onResponse(retrofit2.Call<com.ck.myapplication.Response> call, retrofit2.Response<com.ck.myapplication.Response> response) {
                code = response.body().getCode();
            }
            @Override
            public void onFailure(retrofit2.Call<com.ck.myapplication.Response> call, Throwable t) {
                System.out.println("连接失败");
            }
        });
```

Response.java

```java
package com.ck.myapplication;

/**
 * Created by 27612 on 2018/5/23.
 */

public class Response {
    /**
     * code : 1000
     * message : 登陆成功
     * data : {"user_id":2,"token":"9a37351e6415a1573eb2aa3b20c81f27"}
     */

    private int code;
    private String message;
    private DataBean data;

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public DataBean getData() {
        return data;
    }

    public void setData(DataBean data) {
        this.data = data;
    }

    public static class DataBean {
        /**
         * user_id : 2
         * token : 9a37351e6415a1573eb2aa3b20c81f27
         */

        private int user_id;
        private String token;

        public int getUser_id() {
            return user_id;
        }

        public void setUser_id(int user_id) {
            this.user_id = user_id;
        }

        public String getToken() {
            return token;
        }

        public void setToken(String token) {
            this.token = token;
        }
    }
}

```

RetrofitPost接口

```java
package com.ck.myapplication;
public interface RetrofitPost {

    @Headers({"Content-Type: application/json","Accept: application/json"})
    @POST("login")
    Call<Response> getCall(@Body RequestBody ck);
}

```

