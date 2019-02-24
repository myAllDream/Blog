[TOC]

# 解决hexo d -g卡住不动问题

从家来到学校，打算更新下博客，可他咋就是怎么都不行，一开始我还以为是寝室网不行，换手机网也不行，最后跑到工学馆用校园网，还不行，想也不应该啊，昨天从家还行呢，怎么今天到学校就不行了。最后还是得感谢百度呗。

今天打算整理出来一篇电影博客，带了好多照片，第一回部署的时候有点慢，我就给Ctrl+c了，后来再次部署的时候总是出现卡顿，无反应。

![卡顿](http://r.photo.store.qq.com/psb?/V14L47VC0w3vOf/UgTaFzb5IyXrEVDqXPxlbhZ0ZWbhh55axJ02fiNHl5Y!/r/dDUBAAAAAAAA)

<!--more-->

重新设置SSHkey我都试过了，最后百度到一条，说删除根目录下的.deploy_git文件夹，马上试了试，结果还真部署成功了。

后来我打开后生成的.deploy_git文件夹，里面结构目录和github仓库里的一样，是本地先生成的页面什么东西，然后再整体上传到github上，就是因为那次图片传的有点多我嫌慢暂停后导致这里乱了。删除重新生成就好了。

# 解决页面数学公式渲染错误问题

- 安装新的渲染插件

```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

- 更改主题配置文件的mathjax

```
# MathJax Support
mathjax:
  enable: true
  per_page: false
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config=TeX-AMS-MML_HTMLorMML
```

- `hexo clean`和`hexo g`就OK了