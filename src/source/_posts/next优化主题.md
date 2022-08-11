---
title: next优化主题
top: true
date: 2022-08-08 08:21:51
tags:
---


# next优化主题
主要参考文献：https://zhuanlan.zhihu.com/p/106060640
## 1. 设置菜单
打开主题配置文件即themes/next下的_config.yml，查找menu，将前面的#删除就行了：
```bash
menu:
  home: / || home                      #首页
  archives: /archives/ || archive      #归档
  categories: /categories/ || th       #分类
  tags: /tags/ || tags                 #标签
  about: /about/ || user               #关于
  resources: /resources/ || download   #资源
  #schedule: /schedule/ || calendar    #日历
  #sitemap: /sitemap.xml || sitemap    #站点地图，供搜索引擎爬取
  #commonweal: /404/ || heartbeat      #腾讯公益404
```

“||”前面的是目标链接，后面的是图标名称，
next使用的图标全是[图标库 - Font Awesome 中文网](http://www.fontawesome.com.cn/faicons/)这一网站的,
有想用的图标直接在fontawesome上面找图标的名称就行。resources是我自己添加的。

<!-- more -->

新添加的菜单需要翻译对应的中文，打开theme/next/languages/zh-CN.yml，在menu下设置：
```bash
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  resources: 资源
  search: 搜索
```

增加对应页面
```bash
hexo new page "categories"
hexo new page "tags"
hexo new page "about"
hexo new page "resources"
```

此时在根目录的sources文件夹下会生成categories、tags、about、resources四个文件，
每个文件中有一个index.md文件，修改内容分别如下：
```bash
---
title: 分类
date: 2020-02-10 22:07:08
type: "categories"
comments: false
---

---
title: 标签
date: 2020-02-10 22:07:08
type: "tags"
comments: false
---

---
title: 关于
date: 2020-02-10 22:07:08
type: "about"
comments: false
---

---
title: 资源
date: 2020-02-10 22:07:08
type: "resources"
comments: false
---
```
* 注：如果有启用评论，默认页面带有评论。需要关闭的话，添加字段comments并将值设置为false。


## 2. 设置建站时间

打开主题配置文件即themes/next下的_config.yml，查找since：
```bash
footer:
  # Specify the date when the site was setup. If not defined, current year will be used.
  since: 2020-02   #建站时间
```

## 3. 设置头像
打开主题配置文件即themes/next下的_config.yml，查找avatar，url后是图片的链接地址：
```bash
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: /images/avatar.gif   #图片的位置，也可以是http://xxx.com/avatar.png
  # If true, the avatar will be dispalyed in circle.
  rounded: true   #头像展示在圈里
  # If true, the avatar will be rotated with the cursor.
  rotated: false  #头像随光标旋转
```
然后将你要的头像图片复制到themes/next/source/images里，重命名为avatar.png。

## 4. 网站图标设置

下载16x16和32x32的图标后，打开主题配置文件，查找favicon，只要修改small和medium为你的图标路径：
```bash
favicon:
  small: /images/favicon-16x16.png
  medium: /images/favicon-32x32.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```

## 5. 设置动态背景
### 5.1 canvas nest 风格

在themes/next目录下输入：
```bash
git clone https://github.com/theme-next/theme-next-canvas-nest source/lib/canvas-nest
```
打开主题配置文件即themes/next下的_config.yml，找到canvas-nest，将enable：false改为true：
**（如果找不到canvas-nest，可能是文件修改了，试试将下面的代码复制粘贴到themes/next中）**
```bash
# Canvas-nest
# Dependencies: https://github.com/theme-next/theme-next-canvas-nest
# For more information: https://github.com/hustcc/canvas-nest.js
canvas_nest:
  enable: true
  onmobile: true # Display on mobile or not
  color: "0,0,255" # RGB values, use `,` to separate
  opacity: 0.5 # The opacity of line: 0~1
  zIndex: -1 # z-index property of the background
  count: 99 # The number of lines
```

### 5.2 JavaScript 3D library风格
在themes/next目录下输入：
```bash
git clone https://github.com/theme-next/theme-next-three source/lib/three
```
打开主题配置文件即themes/next下的_config.yml，找到three，这里有三种风格，可以试一下看看喜欢哪种风格，直接将false改为true就行了，我已经选了canvas-nest，就没有选这种风格
```bash
# JavaScript 3D library.
# Dependencies: https://github.com/theme-next/theme-next-three
three:
  enable: true
  three_waves: false
  canvas_lines: false
  canvas_sphere: false
```

## 6. 设置背景图片
打开主题配置文件即themes/next下的_config.yml，将 style: source/_data/styles.styl 取消注释：
```bash
custom_file_path:
  style: source/_data/styles.styl
```
* 注意缩进级别

打开根目录Blog/source创建文件_data/styles.styl，添加以下内容：
```bash
// 添加背景图片
body {
      background: url(/images/background.jpg);//自己喜欢的图片地址
      background-size: cover;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-position: 50% 50%;
}
```
如果main.styl报错找不到相关代码，themes/next/source/css下找到main.styl文件
最后一行的配置修改为，并修改imoort目录
```bash
// Custom Layer
// --------------------------------------------------
for $inject_style in hexo-config('injects.style')
  @import '../_data/styles.styl'; // 修改内容


```


## 7. 主页文章添加阴影效果
打开themes/next/source/css/_common/conponents/post/post.styl，修改.post-block，如下：
```bash
.use-motion {
  if (hexo-config('motion.transition.post_block')) {
    .post-block {
      opacity: 0;
      margin-top: 60px;
      margin-bottom: 60px;
      padding: 25px;
      background:rgba(255,255,255,0.9) none repeat scroll !important;
      -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
      -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);

    }
    .pagination, .comments{
      opacity: 0;
    }
  }
```

还有一种方法打开Blog/source/_date/style.styl文件，添加以下代码：
```bash
// 主页文章添加阴影效果
.post-block {
  margin-top: 60px;
  margin-bottom: 60px;
  padding: 25px;
  background:rgba(255,255,255,0.6) none repeat scroll !important;
  -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
  -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
}
```
* 有错误待验证

## 8. 添加顶部加载条
在themes/next目录下，输入：
```bash
git clone https://github.com/theme-next/theme-next-pace source/lib/pace

```

打开**主题配置文件**即themes/next下的_config.yml，
找到pace，将enable：false改为true，你还可以选择类型（theme）：
```bash
pace:
  enable: true
  # Themes list:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal
```

## 9. 设置预览摘要
next（v7.7.1）已经没有如下代码：
```bash
auto_excerpt:
  enable: true
  length: 150
```

所以不需要设置，只要我们在文章中插入 ```<!-- more -->```，该标签之上的是摘要，之后的内容不可见，需点击全文阅读按钮：
```bash
 <!-- more -->
```













## 10. 设置侧边栏显示效果
打开主题配置文件即themes/next下的_config.yml，找到Sidebar Settings，设置：
```bash
sidebar:
  # Sidebar Position. #设置侧边栏位置
  position: left
  #position: right

  #  - post    默认显示模式
  #  - always  一直显示
  #  - hide    初始隐藏
  #  - remove  移除侧边栏
  display: post
```

## 11. 侧边栏推荐阅读
打开主题配置文件即themes/next下的_config.yml，搜索links（里面写你想要的链接）：
```bash
links_settings:
  icon: link
  title: 链接网站  #修改名称
links:
  #Title: http://yoursite.com
  百度: https://baidu.com
  鱼C论坛: https://fishc.com.cn

```

## 12. 添加社交链接
打开主题配置文件即themes/next下的_config.yml，搜索social：
```bash
social:
  GitHub: https://github.com/fengye97 || github
  E-Mail: mailto:yinhongfei1018@163.com || envelope
  知乎: https://www.zhihu.com/people/mai-nv-hai-de-xiao-huo-chai-35-19 || gratipay
  CSDN: https://https://blog.csdn.net/Later_001 || codiepie


```
“||”前面的是链接，后面的是 FontAwesome图标名称。

## 13. 设置博文内链接为蓝色
打开themes/next/source/css/_common/components/post/post.styl文件，将下面的代码复制到文件最后：
```bash
.post-body p a{
     color: #0593d3;
     border-bottom: none;
     &:hover {
       color: #0477ab;
       text-decoration: underline;
     }
   }


```
重新部署一下后，效果如下图：
xxxx


## 14. 显示文章字数和阅读时长
从根目录Blog打开Git Bash，执行下面的命令，安装插件：
```bash
npm install hexo-wordcount --save
npm install hexo-symbols-count-time
```
然后打开站点配置文件Blog/_config.yml，在文件末尾加上下面的代码：

```bash
symbols_count_time:
  symbols: true                # 文章字数统计
  time: true                   # 文章阅读时长
  total_symbols: true          # 站点总字数统计
  total_time: true             # 站点总阅读时长
  exclude_codeblock: false     # 排除代码字数统计

```
* 操作方法也可以参考[github](https://github.com/theme-next/hexo-symbols-count-time) 
效果如下图：
xxxx

## 15. 显示站点文章总字数（另一种统计站点总字数的方法）
从根目录Blog打开Git Bash，执行下面的命令，安装插件：
```bash
npm install hexo-wordcount --save
```
然后在/themes/next/layout/_partials/footer.swig文件尾部加上：
```bash
<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
</div>
```

## 16. 设置文章末尾”本文结束”标记
在路径/themes/next/layout/_macro 中新建 passage-end-tag.swig 文件,并添加以下内容：
```bash
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:24px;">-------------本文结束<i class="fa fa-paw"></i>感谢您的阅读-------------</div>
    {% endif %}
</div>
```
接着打开/themes/next/layout/_macro/post.swig文件，在post-footer前添加代码：
```bash
 {% if not is_index and theme.passage_end_tag.enabled %}
   <div>
     {% include 'passage-end-tag.swig' %}
   </div>
 {% endif %}
```
具体位置如下图：
xxx

然后打开主题配置文件（_config.yml)，在末尾添加：
```bash
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```
完成以上设置之后，在每篇文章之后都会添加如下效果图的样子：


## 17. 文章末尾添加版权说明
查找主题配置文件themes/next/_config.yml中的creative_commons：
```bash
creative_commons:
  license: by-nc-sa
  sidebar: false
  post: true  # 将false改为true即可显示版权信息
  language:
```
效果图：
xxx

## 18. 添加访问量统计
打开主题配置文件即themes/next下的_config.yml，找到busuanzi_count，改为true：
```bash
busuanzi_count:
  enable: true
```
打开/themes/next/layout/_partials/footer.swig，在最后添加如下内容：
```bash
{% if theme.busuanzi_count.enable %}
    <script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>

    <span id="busuanzi_container_site_pv">总访问量<span id="busuanzi_value_site_pv"></span>次</span>
    <span class="post-meta-divider">|</span>
    <span id="busuanzi_container_site_uv">总访客数<span id="busuanzi_value_site_uv"></span>人</span>
    <span class="post-meta-divider">|</span>
<!-- 不蒜子计数初始值纠正 -->
<script>
$(document).ready(function() {

    var int = setInterval(fixCount, 50);  // 50ms周期检测函数
    var countOffset = 20000;  // 初始化首次数据

    function fixCount() {            
       if (document.getElementById("busuanzi_container_site_pv").style.display != "none")
        {
            $("#busuanzi_value_site_pv").html(parseInt($("#busuanzi_value_site_pv").html()) + countOffset); 
            clearInterval(int);
        }                  
        if ($("#busuanzi_container_site_pv").css("display") != "none")
        {
            $("#busuanzi_value_site_uv").html(parseInt($("#busuanzi_value_site_uv").html()) + countOffset); // 加上初始数据 
            clearInterval(int); // 停止检测
        }  
    }
       	
});
</script> 
{% endif %}
```

## 19. 添加评论功能
### 19.1 注册安装
我采用的是来必力，具体过程很简单，[打开官网：Welcome to LiveRe.com](http://livere.com/)

注册账号：


然后发送邮箱验证码，输入验证码验证，完成注册后点击上方安装：


选择免费的现在安装，会弹出一个框让你填网站的信息：


然后获取代码：


将data-uid后的代码复制，然后打开主题配置文件_config.yml，找到livere_uid，将代码复制到其后即可：
```bash
livere_uid: "你的代码"
```

### 19.2 设置提醒
当有新评论出现时，通过邮箱提醒，点击右上角->管理页面->评论提醒：


完成后效果如图：


### 20. 添加Fork me on Github
有两种，分别是：



选择你喜欢的类型，打开[小猫](https://tholman.com/github-corners/)或者[字](https://github.blog/2008-12-19-github-ribbons/)，复制代码添加到themes/next/layout/_layout.swig文件中，放在<div class="headband"></div>后面：
```html
<div class="headband"></div>
    <a href="https://your-url" class="github-corner" aria-label="View source on GitHub"><svg width="80" height="80" viewBox="0 0 250 250" style="fill:#64CEAA; color:#fff; position: absolute; top: 0; border: 0; right: 0;" aria-hidden="true"><path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"></path><path d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2" fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"></path><path d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z" fill="currentColor" class="octo-body"></path></svg></a><style>.github-corner:hover .octo-arm{animation:octocat-wave 560ms ease-in-out}@keyframes octocat-wave{0%,100%{transform:rotate(0)}20%,60%{transform:rotate(-25deg)}40%,80%{transform:rotate(10deg)}}@media (max-width:500px){.github-corner:hover .octo-arm{animation:none}.github-corner .octo-arm{animation:octocat-wave 560ms ease-in-out}}</style>
```

## 21. 安装Markdown编译器
可以看这篇文章然后选一个适合的文本编译器：[几款主流好用的markdown编辑器介绍](https://link.zhihu.com/?target=https%3A//blog.csdn.net/davidhzq/article/details/100815332)

我是用的Windows系统，所以我用的是MarkdownPad2，下载地址：[The Markdown Editor for Windows](https://link.zhihu.com/?target=http%3A//www.markdownpad.com/)，默认安装就行

如果是win10系统还需要安装一个组件 awesomium_v1.6.6_sdk_win，下载链接：链接：[https://pan.baidu.com/s/1UJRtOBF8vj19ikOq4452sQ](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1UJRtOBF8vj19ikOq4452sQ) 提取码：yd8k

下载完awesomium_v1.6.6_sdk_win默认安装就行

## 22. 安装RSS插件
为什么要安装RSS插件呢？不了解的可以看看这篇文章：rss订阅是什么意思?为什么要使用RSS?简单来说，RSS是一种协议，允许网站将其内容或其部分内容提供给其他网站或应用程序。通过使用RSS，可以节省宝贵的时间，并将各个站点提供的新闻和信息组织到一个中心点进行查看，也可以通过从使用RSS联合其内容的其他站点导入新闻来向你的站点添加新闻。

### （1）安装hexo-generator-feed插件
RSS需要有一个Feed链接，而这个链接需要靠hexo-generator-feed插件来生成，所以第一步需要添加插件，在Blog根目录打开Git Bash执行安装指令：
```bash
npm install hexo-generator-feed --save
```
### （2）配置feed信息
在站点配置文件末尾加上如下代码：
```bash
feed:
  type: rss2
  path: rss2.xml
  limit: 10
  hub:
  content: 'true'
```
### （3）配置rss
打开主题配置文件，搜索rss，将前面的#去掉：
```bash
follow_me:
  #Twitter: https://twitter.com/username || twitter
  #Telegram: https://t.me/channel_name || telegram
  微信: /images/wechat_channel.jpg || wechat
  RSS: /atom.xml || rss
```
效果如图：
xxx



## 23. 项目主页添加README
在建立Github上建立自己的博客仓库的时候，没有生成README文件，
这样使得其他人无法知道我们这个仓库是做什么，即我们的这个仓库缺少一个说明文件；
但是如果直接使用命令hexo n README，再部署至远程的时候，
hexo会将它解析为html文件，这不是我们想要的。

因此，解决方式是在路径Blog\source下手动新建README.md注意这里的后缀是.mdown，
Mardownpad2可以将文件保存为.mdown后缀文件；然后再在这个新建文件中写README即可。
再之后hexo g会把它复制到public文件夹，而不会被解析成html文件，发布在博客中。

也可以参考第24项，在根目录Blog/source 目录下添加一个 README.md 文件，修改站点配置文件 _config.yml ，将 skip_render 参数的值设置为:
```bash
skip_render: README.md
```
## 24. 忽略要编译的文件
搜索引擎确认网站所有权时往往会提供一个html文件来进行验证，
要是这个文件被渲染了，验证自然就会失败了。
或者，有时候会有一些文件不希望Hexo渲染的，
因此有必要针对某个文件或者目录进行排除。
Hexo的配置文件中提供了配置项skip_render。
只有source目录下的文件才会发布到public（能够在网络上访问到），
因此Hexo只渲染source目录下的文件。
skip_render参数设置的路径是相对于source目录的路径。
```bash
skip_render:   #部署时不包含的文件

#单个文件
skip_render: hello.html

#单个文件夹下全部文件
skip_render: test/* 

#单个文件夹下指定类型文件
skip_render: test/*.md  

#单个文件夹下全部文件以及子目录
skip_render: test/**  

#跳过多个目录，或者多个文件
skip_render: ['*.html', demo/**, test/*]
```

## 25. 代码块样式自定义
打开根目录Blog/source/_data/styles.styl，添加以下内容：
```bash
// Custom styles.
code {
    color: #ff7600;
    background: #fbf7f8;
    margin: 2px;
}
// 大代码块的自定义样式
.highlight, pre {
    margin: 5px 0;
    padding: 5px;
    border-radius: 3px;
}
.highlight, code, pre {
    border: 1px solid #d6d6d6;
}
```
效果图：
xxx

## 26. 支持mathjax公式
打开主题配置文件，搜索mathjax：
```bash
mathjax:
    enable: true   #将false改为true
    mhchem: false  #用来写化学方程式
```

## 27. 图床网站
推荐七牛云，免费网站，快速，方便，或者可以看看这篇文章选一个合适的网站：[软件小编：盘点一下免费好用的图床](https://zhuanlan.zhihu.com/p/35270383)

网站：[七牛云](https://link.zhihu.com/?target=https%3A//www.qiniu.com/%3Futm_campaign%3DSEM%26utm_content%3Dpinzhuan%26utm_medium%3Dpinzhuan%26utm_source%3DbaiduSEM%26utm_term%3Dpinzhuan)，后面我再写一篇关于如何注册使用七牛云存储图片的文章吧，我先研究研究。

## 28. 本地搜索
打开cmd安装插件：
```bash
npm install hexo-generator-search
```
查找主题配置文件themes/next/_config.yml中的local_search ：
```bash
local_search:
  enable: true
  trigger: manual   #手动，按回车键或搜索按钮触发搜索
```

## 29. Hexo博客提交百度和Google收录
这篇文章写得很详细：[Hexo博客提交百度和Google收录](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/f8ec422ebd52)

## 30. 博文置顶
### （1）安装插件
在根目录Blog打开Git Bash，执行下面的命令：
```bash
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

### （2）设置置顶标志
打开blog/themes/next/layout/_macro目录下的post.swig文件，
定位到```<div class="post-meta">```标签下，插入如下代码：
```html
{% if post.top %}
  <i class="fa fa-thumb-tack"></i>
  <font color=7D26CD>置顶</font>
  <span class="post-meta-divider">|</span>
{% endif %}
```


### （3）在文章中添加top
然后在需要置顶的文章的Front-matter中加上top: true即可，如下：
```yaml
---
title: Hello World

top: true

---
```
效果如图：
XXX

## 31. 添加打赏
next（v7.7.1）已经有打赏功能，只要准备好微信和支付宝二维码，具体操作：制作微信二维码收款，很简单我就不详细说了。

打开主题配置文件，查找reward：
```yaml
reward_settings:
  enable: true   #改为true
  animation: false
  comment: 原创技术分享，您的支持将鼓励我继续创作


reward:
  wechatpay: /images/wechatpay.png   #将前面的#去掉
  alipay: /images/alipay.png         #将前面的#去掉
  #paypal: /images/paypal.png
  #bitcoin: /images/bitcoin.png


```
将制作好的二维码图片放入themes/next/source/images文件里，并命名为wechatpay.png和alipay.png。

## 32. 添加分享（未完成）
原来的 jiathis share 和 baidushare 已经没有了，现在的是 addthis，但是我注册不了addthis账号 [AddThis官网](https://link.zhihu.com/?target=https%3A//www.addthis.com/)

不知道什么原因，我用Firefox不行，下次用Chrome试试，先放着吧！

## 33. 图片可点击放大查看，放大后可关闭（fancybox可能有点问题，暂时未实现）
打开主题配置文件_config.yml，搜索fancybox，设置其值为true：
```bash
fancybox: true
```
进入到theme/next文件夹下，打开Git Bash，输入如下代码：
```bash
git clone theme-next/theme-next-fancybox3 source/lib/fancybox
```
