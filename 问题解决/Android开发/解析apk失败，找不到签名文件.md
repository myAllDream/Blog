[TOC]

> 看下面的V1机制和V2机制
>
> 把**散列值换成一段数据的指纹**（SHA-256），那么对数据指纹**加密的结果**实际上就是其**数字签名**。
>
> V1： 
>
> - 二次签名，非对称加密，链式解析【比较指纹】，
> - 文件中存的是指纹，解析出来后也和指纹对比
>
> - 完整性、META-INF
>
> V2：
>
> - 7.0引入、对app文件更改进行保护，全文件签名方案
> - v2 签名模式在原先 APK 块中增加了一个新的块（签名块），共四块
> - 四大块按1M分成多个小块，小块计算摘要，然后再计算大摘要，然后将 APK 的摘要 + 数字证书 + 其他属性生成签名数据写入到 APK Signing Block 区块。
> - 不可绕过V2，有则必须验证
>
> 只勾选V2，Android7.0以下的机型会报错
>
> v1机制与v2机制，验证机制，7.0下会忽略v2，如何选择

### 一、前言

> 记一次惨痛的android打包经历



### 二、经过

东大安保上线在即，想法给教主试一试，发过去两遍都是安装失败，期初是想自己打包的肯定没问题啊，之前不洗碗助手也都是这样打包的，也没人反应有问题啊，可能大家年轻人吧，都是android7.0以上了，教主的6.1的系统，都赖我改了minVersionSdk还是不行，再再再后来，打算上传到百度开发者中心，结果一直提示解析apk失败，找不到签名文件，网上一顿乱搜索

被简书上[一篇文章](https://www.jianshu.com/p/e38dc69b90b1)救了命。



### 三、解决

V2这种签名方案是Android7.0引入的，它能提供**更快的应用安装时间和更多针对未授权 APK 文件更改的保护**。具体请看[这里](https://developer.android.google.cn/about/versions/nougat/android-7.0#apk_signature_v2)。V1适用于所有android版本的机型，但在Android7.0及以上会缺少针对未授权 APK 文件更改的保护；只勾选V2，Android7.0以下的机型会报错，所以这里建议同时勾选V1，V2，以适用所有机型。

![v1v2](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/7BdVyyJbPMi*sIX5B.nr*vppvdwZxvTLlPXNsychJzw!/r/dMAAAAAAAAAA)



正确签名后META-INF文件夹下会有了CERT.RSA。另外，你还可以使用命令行查看RSA文件的内容：keytool -printcert -file path/to/CERT.RSA，在 `META-INF` 文件夹下有三个文件：`MANIFEST.MF`、`CERT.SF`、`CERT.RSA`。它们就是签名过程中生成的文件

- MANIFEST.MF

  该文件中保存的内容其实就是逐一遍历 APK 中的所有条目，如果是目录就跳过，如果是一个文件，就用 SHA1（或者 SHA256）消息摘要算法提取出该文件的摘要然后进行 BASE64 编码后，作为“SHA1-Digest”属性的值写入到 MANIFEST.MF 文件中的一个块中。该块有一个“Name”属性， 其值就是该文件在 APK 包中的路径。

- CERT.SF

  - SHA1-Digest-Manifest-Main-Attributes：对 MANIFEST.MF 头部的块做 SHA1（或者SHA256）后再用 Base64 编码
  - SHA1-Digest-Manifest：对整个 MANIFEST.MF 文件做 SHA1（或者 SHA256）后再用 Base64 编码
  - SHA1-Digest：对 MANIFEST.MF 的各个条目做 SHA1（或者 SHA256）后再用 Base64 编码
  - 对于 SHA1-Digest 值的验证可以手动进行，将 MANIFEST.MF 中任意一个块的内容复制并保存在一个新的文档中，注意文末需要加两个换行（这是由 signAPK 的源码决定的）

- CERT.RSA

  - 这里会把之前生成的 CERT.SF 文件，**用私钥计算出签名, 然后将签名以及包含公钥信息的数字证书一同写入 CERT.RSA 中保存**。这里要注意的是，Android APK 中的 CERT.RSA **证书是自签名的**，并不需要这个证书是第三方权威机构发布或者认证的，用户可以在本地机器自行生成这个自签名证书。Android 目前不对应用证书进行 CA 认证。



### 四、原因

#### 1、v1与v2机制

**v1签名是对jar进行签名，V2签名是对整个apk签名**：官方介绍就是：v2签名是在整个APK文件的二进制内容上计算和验证的，v1是在归档文件中解压缩文件内容。

v1：在v1中只对**未压缩**的文件内容进行了验证，所以在APK**签名之后**可以进行很多修改——文件可以移动，甚至可以重新压缩。**即可以对签名后的文件在进行处理** 
v2：v2签名验证了归档中的所有字节，而不是单独的ZIP条目，如果您在构建过程中有任何定制任务，包括篡改或处理APK文件，请确保禁用它们，否则您可能会使v2签名失效，从而使您的APKs与Android 7.0和以上版本不兼容。

**一个APK可以同时由v1和v2签名同时签署，所以它仍然可以向后兼容以前的Android版本。**



#### 2、签名验证机制

在 Android 7.0 以上版本的设备上，APK 可以根据Full Apk Signature（v2 方案） 或者 JAR-signed（ v1方案）进行验证； 
而对于7.0以下版本的设备其会忽略 v2 版本的签名，只验证 v1 签名



#### 3、v1与v2如何选择

- 一定可行的方案： 只使用 v1 方案

- **不一定**可行的方案：同时使用 v1 和 v2 方案

- 对 7.0 以下**一定不**行的方案：只使用 v2 方案

1. 如果要支持 Android 7.0 以下版本，那么尽量同时选择两种签名方式，但是一旦遇到签名问题，可以只使用 v1 签名方案

2. 如果需要对签名后的信息做处理修改，那就使用v1签名方案
3. 如果最后遇到各种不同的问题，可以不勾选v1和v2，直接打包签名

  

PS：**如果要支持 Android 7.0 以下版本，那么尽量同时选择两种签名方式，但是一旦遇到签名问题，可以只使用 v1 签名方案**



##### 

### 五、重要概念

#### 1、散列算法

也就是说散列函数（通常也叫散列算法）可以把任意长度的数据映射成固定长度的数据，映射出来的数据称为**散列值**、**哈希值**、**摘要**。因为输入数据不同，得到的散列值不同（很大概率），所以可当做输入数据的**指纹**。常见的散列算法[[2\]](#fn2)有 MD5、SHA-1、SHA-256，以下要介绍的 APK 签名会用到 **SHA-265**[[3\]](#fn3)散列算法。

#### 2、加密

加密就是把可读的**明文数据**通过加密算法转换成不可读的**密文数据**，只有通过相应的解密算法才能把密文数据转换成明文数据，常见的加密算法分为**对称加密**和**非对称加密**。

**对称加密就是加密和解密都用同一把“钥匙”。**

- DES、3DES、AES

举个栗子，小明有一把**普通锁**，这把锁能且只能用同一把钥匙（暂且称为 K ）上锁和开锁：假如小明用钥匙 K 上了锁，他的朋友小红要打开这个锁，那么只能事先让小明配一把一样的钥匙 K' 给她。

**非对称加密就是加密和解密用的是两把不同的“钥匙”。**

- RSA

以下要介绍的 APK 签名会用到使用最广泛的非对称加密算法 —— **RSA**。

#### 3、数字签名

数字签名就是证明数据真实性的一种方式。

> 把**散列值换成一段数据的指纹**（SHA-256），那么对数据指纹**加密的结果**实际上就是其**数字签名**。



### 六、V1机制【链式】

#### 1、签名工具

APK 签名可以用 jarsigner 或者 signapk 两个工具，Android Studio 默认用的是 **signapk**，二者主要的区别在于证书和秘钥存储的格式不同，前者是通过 Java KeyStore（**.jks 文件或者 .keystore 文件）** 格式，后者分别用 .pem 和 .pk8 格式来存储证书和密钥。

- Java KeyStore 生成方式：
  - 【生成】证书库
    keytool -genkey -v -keystore strange.keystore -alias strange -keyalg RSA -keysize 2048 -validity 10000
  - 【**查看**】证书库
    keytool -list -v -keystore {path2jks} -storepass “pass"

- pem .pk8 生成方式：
  - 【生成】密钥
    openssl genrsa -out key.pem 2048
  - 【生成】证书请求
    openssl req -new -key key.pem -out request.pem
  - 【生成】 pem格式的 x.509 证书
    openssl x509 -req -days 10000 -in request.pem -signkey key.pem -out certificate.pem -sha256
  - 【生成】 pk8 格式密钥
    openssl pkcs8 -topk8 -outform DER -in key.pem -inform PEM -out key.pk8 -nocrypt
  - 【**查看**】pem证书
    openssl x509 -in publicKey.x509.pem -text -noout

无论是用的哪种签名方式，最终都是在 **META-INF** 目录下生成三个文件：**MANIFEST.MF**、**CERT.SF**、**CERT.RSA**（如果是 jarsigner 签名 .SF 和 .RSA 文件名会根据 alias 来定），这三个文件各司其职，最终构成了 APK 签名信息。

#### 2、签名与校验过程

##### 签名过程

![签名过程](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/YTLwyNeoLA1BWMoXdbh*hEbJ12.XFgi6UDIJE4oY9OQ!/r/dFQBAAAAAAAA)





---



##### 校验过程

![校验过程](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/IS1lwGQfbFvY8BvEybrADHP7gE7u0.4BbknneioMcaM!/r/dMEAAAAAAAAA)

校验在App安装时进行

1. 检查 APK 中包含的所有文件，对应的摘要值与 MANIFEST.MF 文件中记录的值一致。
2. 使用证书文件（RSA 文件）检验签名文件（SF 文件）没有被修改过。
3. 使用签名文件（SF 文件）检验 MF 文件没有被修改过。

---



#### 3、总结

最后总结一下 MANIFEST.MF、CERT.SF、CERT.RSA 如何各司其职构成了 APK 的签名：

- 解析出 CERT.RSA 文件中的证书、公钥，解密 CERT.RSA 中的加密数据
- 解密结果和 CERT.SF 的**指纹**进行对比，保证 CERT.SF 没有被篡改
- 而 CERT.SF 中的内容再和 MANIFEST.MF **指纹**对比，保证 MANIFEST.MF 文件没有被篡改
- MANIFEST.MF 中的内容和 APK 所有文件**指纹逐一对比**，**保证 APK 没有被篡改**



### 七、V2机制

APK 签名方案 v2 是一种全文件签名方案，该方案能够发现对 APK 的受保护部分进行的所有更改，从而有助于加快验证速度并增强完整性保证。

#### 1、v1 签名机制的劣势

从 Android 7.0 开始，Android 支持了一套全新的 V2 签名机制，为什么要推出新的签名机制呢？通过前面的分析，可以发现 v1 签名有两个地方可以改进：

- 签名校验速度慢
  校验过程中需要对apk中**所有文件**进行摘要计算，在 APK 资源很多、性能较差的机器上签名校验会花费较长时间，导致安装速度慢。

- 完整性保障不够
  META-INF 目录用来存放签名，自然此目录本身是不计入签名校验过程的，可以随意在这个目录中添加文件，比如一些快速批量打包方案就选择在这个目录中添加渠道文件。

为了解决这两个问题，在 Android 7.0 Nougat 中引入了全新的 APK Signature Scheme v2。

#### 2、v2 带来了什么变化？

由于在 v1 仅针对单个 ZIP 条目进行验证，因此，在 APK 签署后可进行许多修改 — 可以移动甚至重新压缩文件。事实上，编译过程中要用到的 ZIPalign 工具就是这么做的，它用于根据正确的字节限制调整 ZIP 条目，以改进运行时性能。而且我们也可以利用这个东西，**在打包之后修改 META-INF 目录下面的内容，或者修改 ZIP 的注释来实现多渠道的打包，在 v1 签名中都可以校验通过。**

v2 签名将验证归档中的所有字节，而不是单个 ZIP 条目，因此，在签署后无法再运行 ZIPalign（必须在签名之前执行）。正因如此，现在，在编译过程中，Google 将压缩、调整和签署合并成一步完成。

#### 3、v2 签名模式

简单来说，v2 签名模式在原先 APK 块中增加了一个新的块（签名块），**新的块存储了签名，摘要，签名算法，证书链，额外属性等信息**，这个块有特定的格式，具体格式分析见后文，先看下现在 APK 成什么样子了。

为了保护 APK 内容，整个 APK（ZIP文件格式）被分为以下 4 个区块：

- ZIP 条目的内容（从偏移量 0 处开始一直到“APK 签名分块”的起始位置）
- APK 签名分块
- ZIP 中央目录
- ZIP 中央目录结尾

![v2包内容](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/AAljffMZhgETnjOAFbcpcnBGm7ubsWg9olIgUhyQTNI!/r/dFIBAAAAAAAA)

其中，应用签名方案的签名信息会被保存在 区块 2（APK Signing Block）中，而区块 1（Contents of ZIP entries）、区块 3（ZIP Central Directory）、区块 4（ZIP End of Central Directory）是受保护的，在签名后任何对区块 1、3、4 的修改都逃不过新的应用签名方案的检查。

#### 4、签名过程

![v2签名过程](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/IeOVthfvtaEBw909.MAMDoGekO7H7fTmJ7DxhP1oANE!/r/dFYBAAAAAAAA)

APK 摘要计算规则，对于每个摘要算法，计算结果如下:

- 将 APK 中文件 ZIP 条目的内容、ZIP 中央目录、ZIP 中央目录结尾按照 1MB 大小分割成一些小块。
- 计算每个小块的数据摘要，数据内容是 0xa5 + 块字节长度 + 块内容。
- 计算整体的数据摘要，数据内容是 0x5a + 数据块的数量 + 每个数据块的摘要内容

总之，就是把 APK 按照 1M 大小分割，分别计算这些分段的摘要，最后把这些分段的摘要在进行计算得到最终的摘要也就是 APK 的摘要。然后将 APK 的摘要 + 数字证书 + 其他属性生成签名数据写入到 APK Signing Block 区块。

#### 5、验证过程

![v2校验过程](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/GaUxHvjfK2Zejl8UEHkjx5Ge5tFnsVL3JCpOyVKVsX8!/r/dLYAAAAAAAAA)



其中 v2 签名机制是在 **Android 7.0 以及以上版本**才支持。因此对于 Android 7.0 以及以上版本，在安装过程中，如果发现有 v2 签名块，则必须走 v2 签名机制，不能绕过。否则降级走 v1 签名机制。
v1 和 v2 签名机制是可以同时存在的，其中对于 v1 和 v2 版本同时存在的时候，v1 版本的 META_INF 的 .SF 文件属性当中有一个 X-Android-APK-Signed 属性：

```java
X-Android-APK-Signed: 2
```



### 链接

- [一文看懂android签名机制](https://blog.csdn.net/freekiteyu/article/details/84849651)

- [Android APK V1 签名原理](https://www.jianshu.com/p/a27783a713f2)