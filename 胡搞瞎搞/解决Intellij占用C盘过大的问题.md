

您要是c盘足够大就不用啦~哈哈

最近，哦哦不是最近，就发现电脑C盘的空间越来越小，弄得我都想重装系统了，哎，当时傻傻的买个电脑128固态还被拆分成了两部分，弄的我c盘好小好小。

发现原来占空间的也就是JetBrains家的东西，一些以.开头的大概是什么配置文件的东西吧。

AS是基于Idea Intellij平台开发的，非常稳定，并且扩展性比较好，由Idea Intellij衍生出来的平台还有很多，比如用来开发python项目的Pycharm，用于开发PHP项目的PHPStorm等，这一类的工具有一个特性，即会对项目文件构建索引，便于查阅，并且编译速度快，但也有一个缺点，就是假如项目比较大，那么相应的索引文件也比较大，有的多达几个G，而索引文件默认存储的目录是在系统盘，假如电脑硬盘较小(比如当前很多电脑，为了兼顾速度和性价比，选择容量较小的SSD)，那会造成严重的隐患，所以，安装好AS之后，建议修改配置文件。 

- 找到JetBrains产品的安装目录找到bin文件夹找到**idea.properties**文件，记事本打开

```wiki
#---------------------------------------------------------------------
# Uncomment this option if you want to customize path to IDE config folder. Make sure you're using forward slashes.
#---------------------------------------------------------------------
# idea.config.path=${user.home}/.AndroidStudio/config

#---------------------------------------------------------------------
# Uncomment this option if you want to customize path to IDE system folder. Make sure you're using forward slashes.
#---------------------------------------------------------------------
# idea.system.path=${user.home}/.AndroidStudio/system

#---------------------------------------------------------------------
# Uncomment this option if you want to customize path to user installed plugins folder. Make sure you're using forward slashes.
#---------------------------------------------------------------------
# idea.plugins.path=${idea.config.path}/plugins

#---------------------------------------------------------------------
```

**注意：**这些文件里#开头的是注释

将 `${user.home}`和`${idea.config.path}`修改成你想要变成的路径就行了。

至于我的IDEA是破解版的，只是更改路径就重新打开后貌似不行，我就把原来的配置文件复制过来了就行了。



**Android Studio：**

- .AndroidStudio是AndroidStudio 配置与插件缓存文件夹 
- .gradle 这个文件夹一般不会增长太多，其中存储的是本地的gradle全局配置文件 
- .m2 这个是本地仓库地址，也就是你使用的所有的远程库都会先缓存到这里然后再添加到你的项目中进行使用；如果你用的插件越多这个文件夹将会持续增大 
- .android是存放ADV的地方，非常占地方

---

**2018年7月15更新**

## 解决更改配置文件后插件不能安装问题

**解决办法来自网络**

很多人修改AS的配置文件后，插件都会出问题，比如安装新插件，AS重启后，插件并没有安装上， 
 在idea.properties中，插件默认的路径是：

> idea.plugins.path=${idea.config.path}/plugins

修改配置文件后，新下载的插件却被保存到${idea.system.path}/plugins目录下，这就导致AS重启后找不到插件文件，故而无法正常显示插件，所以，需要将插件目录重新设定：

> idea.plugins.path=${idea.system.path}/plugins

**并且，此时如果插件只是单独的,jar文件，那么没有问题，但如果下载的插件是.zip文件，那就需要手动解压文件，重启AS后，插件才能正常显示。**

解压之后还需要把lib提取出来放到上一级目录中才行，**都是坑啊，都是坑 。**
 下面以.ignore为例，下载后，在目录中是idea-gitignore-1.7.4.zip,需要手动解压文件,解压后，设定此插件的目录如下：

> D:\Local\Android\cache.AndroidStudio\system\plugins\idea-gitignore-1.7.4\lib\idea-gitignore-1.7.4.jar 