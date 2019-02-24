[TOC]



Binder **工作原理**：

- 服务器端：在服务端创建好了一个Binder对象后，内部就会开启一个线程用于接收Binder驱动发送的消息，收到消息后会执行onTranscat()，并按照参数执行不同的服务端代码。
- Binder驱动：在服务端成功Binder对象后，Binder驱动也会创建一个mRemote对象（也是Binder类），客户端可借助它调用transcat()即可向服务端发送消息。
- 客户端：客户端要想访问Binder的远程服务，就必须获取远程服务的Binder对象在Binder驱动层对应的mRemote引用。当获取到mRemote对象的引用后，就可以调用相应Binder对象的暴露给客户端的方法。



AIDL的**本质**是系统提供了一套可快速实现Binder的工具。关键类和方法：

- **AIDL接口**：继承**IInterface**。

- **Stub类**：Binder的实现类，服务端通过这个类来提供服务。

- **Proxy类**：服务器的本地代理，客户端通过这个类调用服务器的方法。

- **asInterface()**

  ：客户端调用，将服务端的返回的Binder对象，转换成客户端所需要的AIDL接口类型对象。返回对象：

  - 若客户端和服务端位于同一进程，则直接返回Stub对象本身；
  - 否则，返回的是系统封装后的Stub.proxy对象。

- **asBinder()**：根据当前调用情况返回代理Proxy的Binder对象。

- **onTransact()**：运行服务端的Binder线程池中，当客户端发起跨进程请求时，远程请求会通过系统底层封装后交由此方法来处理。

- **transact()**：运行在客户端，当客户端发起远程请求的同时将当前线程挂起。之后调用服务端的onTransact()直到远程请求返回，当前线程才继续执行。





### 链接

- [minmin的总结，很全面](https://www.jianshu.com/p/1c70d7306808)
- [Carson_Ho](https://www.jianshu.com/p/4ee3fd07da14)