[TOC]

## 大致步骤

- 安装Git

- 安装Node.js,配置环境变量

- 创建远程GitHub仓库“ck76.github.io”

- 安装Hexo

   ```
    npm install hexo -g
   ```
   <!--more-->

- 新建Blog文件夹，hexo初始化

   ```
    hexo init
   ```

- 安装所需组件

   ```
   npm install
   ```

- 配置站点配置文件与远程仓库连接

   ```
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repository: https://github.com/ck76/ck76.github.io
   ```

- 生成及部署博客

   之前需要先安装一个依赖

    ```
    npm install hexo-deployer-git --save
    ```

  - 部署

    ```
    hexo d -g
    ```

- 安装图片生成插件

   ```
   npm install hexo-asset-image --save
   ```

   Make sure `post_asset_folder: true` in your `_config.yml`.

   Just use `![logo](logo.jpg)` to insert `logo.jpg`.

## 个性化配置

### 在右上角或者左上角实现fork me on github

点击[这里](https://github.com/blog/273-github-ribbons) 或者 [这里](http://tholman.com/github-corners/)挑选自己喜欢的样式，并复制代码。 

然后粘贴刚才复制的代码到`themes/next/layout/_layout.swig`文件中(放在`<div class="headband"></div>`的下面)，并把`href`改为你的github地址 

### 添加动态背景

如果next主题在5.1.1以上的话就不用我这样设置，直接在主题配置文件中找到canvas_nest: false，把它改为canvas_nest: true就行了。

### 侧边栏社交小图标设置

打开主题配置文件（`_config.yml`），搜索`social_icons:`,在[图标库](http://fontawesome.io/icons/)找自己喜欢的小图标，并将名字复制在如下位置 

```tiki wiki
#社交网站地址及图标设置
social:
  #GitHub: https://github.com/yourname || github
  GitHub: https://github.com/ck76 || github                                                                                            #GitHub设置
  微博: https://weibo.com/3309021645/profile?rightmod=1&wvr=6&mod=personinfo || weibo         #微博设置
  豆瓣: https://www.douban.com/people/122105354/                                                              #豆瓣设置
```

### 网站底部字数统计

切换到根目录下，然后运行如下代码

```
$ npm install hexo-wordcount --save1
```

然后在`/themes/next/layout/_partials/footer.swig`文件尾部加上：

```
<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
</div>
```

### 实现统计功能

在根目录下安装 `hexo-wordcount`,运行：

```
$ npm install hexo-wordcount --save1
```

然后在主题的配置文件中，配置如下：

```
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
```

### 添加访问量统计功能

打开`\themes\next\layout\_partials\footer.swig`文件,在copyright前加上画红线这句话： 

```html
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

然后再合适的位置添加显示统计的代码 ,我是加在了

### 博文置顶

修改 `hero-generator-index` 插件，把文件：`node_modules/hexo-generator-index/lib/generator.js` 内的代码替换为：

```javascript
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

在文章中添加 `top` 值，数值越大文章越靠前，如

```markdown
---
title: 
date: 2017-05-22 22:45:48
tags: 
categories: 
copyright: true
top: 100
---
```

## \-------------------------------------

### 目录结构

.
 ├── .deploy       #需要部署的文件
 ├── node_modules  #Hexo插件
 ├── public        #生成的静态网页文件
 ├── scaffolds     #模板
 ├── source        #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
 |   ├── _drafts   #草稿
 |   └── _posts    #文章
 ├── themes        #主题
 ├── _config.yml   #全局配置文件
 └── package.json

### 全局配置 _config.yml

```sh
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/
# Site #站点信息
title:  #标题
subtitle:  #副标题
description:  #站点描述，给搜索引擎看的
author:  #作者
email:  #电子邮箱
language: zh-CN #语言
# URL #链接格式
url:  #网址
root: / #根目录
permalink: :year/:month/:day/:title/ #文章的链接格式
tag_dir: tags #标签目录
archive_dir: archives #存档目录
category_dir: categories #分类目录
code_dir: downloads/code
permalink_defaults:
# Directory #目录
source_dir: source #源文件目录
public_dir: public #生成的网页文件目录
# Writing #写作
new_post_name: :title.md #新文章标题
default_layout: post #默认的模板，包括 post、page、photo、draft（文章、页面、照片、草稿）
titlecase: false #标题转换成大写
external_link: true #在新选项卡中打开连接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
highlight: #语法高亮
  enable: true #是否启用
  line_number: true #显示行号
  tab_replace:
# Category & Tag #分类和标签
default_category: uncategorized #默认分类
category_map:
tag_map:
# Archives
2: 开启分页
1: 禁用分页
0: 全部禁用
archive: 2
category: 2
tag: 2
# Server #本地服务器
port: 4000 #端口号
server_ip: localhost #IP 地址
logger: false
logger_format: dev
# Date / Time format #日期时间格式
date_format: YYYY-MM-DD #参考http://momentjs.com/docs/#/displaying/format/
time_format: H:mm:ss
# Pagination #分页
per_page: 10 #每页文章数，设置成 0 禁用分页
pagination_dir: page
# Disqus #Disqus评论，替换为多说
disqus_shortname:
# Extensions #拓展插件
theme: landscape-plus #主题
exclude_generator:
plugins: #插件，例如生成 RSS 和站点地图的
- hexo-generator-feed
- hexo-generator-sitemap
# Deployment #部署，将 lmintlcx 改成用户名
deploy:
  type: git
  repo: 刚刚github创库地址.git
  branch: master
```

`注意`

- 配置文件的冒号“:”后面有一个空格
- repo: 刚刚github创库地址.git



### hexo命令行使用

常用命令：

```
hexo help #查看帮助
hexo init #初始化一个目录
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成网页，可以在 public 目录查看整个网站的文件
hexo server #本地预览，'Ctrl+C'关闭
hexo deploy #部署.deploy目录
hexo clean #清除缓存，**强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹**
```

简写：

```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

 域名

### 将独立域名与GitHub Pages的空间绑定

方法一：在站点source目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名，比如wuxiaolong.me
 方法二：在Repository的根目录下面，新建一个名为CNAME的文本文件，里面写入你要绑定的域名，比如wuxiaolong.me

## 站点配置文件详解

### 配置

您可以在 _config.yml 中修改大部份的配置。

#### 网站

```
参数              描述
title           网站标题
subtitle        网站副标题
description     网站描述
author          您的名字
language        网站使用的语言
timezone        网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。1234567
```

其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

#### 网址

```
参数                 描述                       默认值
url                  网址     
root               网站根目录    
permalink         文章的永久链接格式      :year/:month/:day/:title/
permalink_defaults  永久链接中各部分的默认值    12345
```

#### 目录

```
参数                 描述   
source_dir       资源文件夹，这个文件夹用来存放内容。     
public_dir       公共文件夹，这个文件夹用于存放生成的站点文件。 
tag_dir          标签文件夹  
archive_dir      归档文件夹  
category_dir     分类文件夹  
code_dir         Include code 文件夹   
i18n_dir         国际化（i18n）文件夹   
skip_render      跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。123456789
```

#### 文章

```
参数                  描述  
new_post_name       新文章的文件名称    
default_layout      预设布局    
auto_spacing        在中文和英文之间加入空格    
titlecase           把标题转换为 title case   
external_link       在新标签中打开链接 
filename_case       把文件名称转换为 (1) 小写或 (2) 大写     
render_drafts       显示草稿    
post_asset_folder   启动 Asset 文件夹    
relative_link       把链接改为与根目录的相对位址  
future              显示未来的文章     
highlight           代码块的设置123456789101112
```

#### 分类&标签

```
参数                  描述          默认值
default_category    默认分类    uncategorized
category_map        分类别名    
tag_map             标签别名    1234
```

#### 时间/日期格式

```
参数              描述      默认值
date_format     日期格式    YYYY-MM-DD
time_format     时间格式    H:mm:ss123
```

#### 分页

```
参数          描述                              默认值
per_page    每页显示的文章量 (0 = 关闭分页功能)   10
pagination_dir  分页目录                        page123
```

#### 扩展

```
参数          描述
theme       当前主题名称。值为false时禁用主题
deploy      部署部分的设置
  type: git
  repository: https://github.com/ck76/ck76.github.io
```





https://www.jianshu.com/p/701b1095da11