---
title: hexo的安装使用
date: 2021-06-14 09:42:14
author: lovelves
avatar: https://cdn.jsdelivr.net/gh/lingdas/note/img/avactor.jpeg
authorLink: 
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
tags: hexo
keywords: hexo
description: hexo安装 初步使用心得
photos: https://cdn.jsdelivr.net/gh/lingdas/note/img/17.jpg
---
## hexo安装及使用 
### 安装前必备环境
大前提是要安装nodejs 和git
**git的下载地址** https://git-scm.com/downloads （git安装对应的版本就可以了 安装好简单 这就不叙述了）
**node下载地址** https://nodejs.org/dist/ 
由于每个人的电脑环境是不一样的 打开这个网站选择自己电脑适合的版本下载
**msi** 后序结尾的是windows版本，
**pkg**结尾的是mac版本 ，
**zip**结尾的是绿色版 
**gz**结尾的事linux版本
{% asset_img 1624018255714.png %}

安装很简单 下一步下一步完成就可以了 就是绿色版比较麻烦 需要自己配置环境变量 建议选择12版本 不高不低  高版本只支持win10
**安装后的第一件事**
给npm 换源
由于国内网络环境 只能换源才能提高速度
使用npm 淘宝镜像（http://npm.taobao.org/）可在cmd命令窗口执行：
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
安装完后 以后用cnpm 来代替npm
以上都安装好后可以 安装hexo了
### 安装hexo
在cmd命令窗口执行以下命令
```bash
cnpm install hexo-cli -g
```
### hexo  使用方法
生成hexo 官方的简单主题
**初始化生成hexo相关文件**
在命令行窗口输入以下命令
```bash
mkdir hello
hexo init  
```
{% asset_img 1624017544903.png %}
在hello文件中会生成以下目录
{% asset_img 1624019373492.png %}

**生成静态页面**
在命令行窗口输入以下命令
```bash
hexo g  
```
{% asset_img 1624019568293.png %}

**运行服务**
```bash
hexo s 
```
{% asset_img 1624019621548.png %}
**访问浏览** http://localhost:4000
{% asset_img 1624019694400.png %}
一个简单的博客网站就搭建完成了。挺简单的

### 网站中的相关目录详细介绍
**_config.yml** 配置文件 配置站点相关信息
```yaml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site 网站相关信息
title: Hexo.  # 网站标题
subtitle: ''
description: ''
keywords:  
author: John Doe #作者
language: en
timezone: ''

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: http://example.com # 部署的网站地址
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false # 建议为ture。
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime'

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape #修改主题

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: ''

```
**配置文件比较重要的属性**
**theme 属性**  网上下载主题后放到根目录的themes文件夹中 然后把主题名字写到这里
比如 我用的是Sakura 主题
{% asset_img 1624023371865.png %}
在配置文件中修改为
{% asset_img 1624023406129.png %}

**post_asset_folder 属性**
这个属性为 true 方便文章中插入图片 当你新建文章时 会自动生成文章对应的文件夹用于存放图片然后在文章中用 
```markdown
{% asset_img 图片1.jpg   图片描述 %} 
```
插入图片 图片路径引用文章对应的文件夹
如果想用回md插入图片语法 可以在配置文件这样配置
```markdown
post_asset_folder: true
marked:
  prependRoot: true
  postAsset: true
```
这样配置后 md中插入图片就简单了 直接写图片的名字如
```markdown
![](图片.png)
```

**deploy 属性**
定义部署的git地址
以github为例
```yaml
deploy:
  type: git
  repo: 
    # github: git@github.com:honjun/honjun.github.io.git
    # github: https://github.com/honjun/honjun.github.io.git
    coding: git@github.com:lingdas/lingdas.github.io.git
  branch: master
```
**public**  程序生成的静态页面 但你执行 hexo g 的时候 会生成
**themes** 存放主题的地方 主题放好了记得在_config.yml 中修改theme属性
**scaffolds** 模版 当你执行命令 hexo n 时候 它默认生成 _post中的模版
**source** 最重要的一个文件 放你的文章 其他文件可以丢 这个文件可不能丢 都是你的心血

### 日常经常使用的hexo命令
**清除已生成的静态文件** 
这个步骤是 配置文件发生变化  或在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。
```bash
hexo clean
```
**生成静态文件** 当新增文章 文章变动时使用它
```bash
hexo g
```
**运行服务** 在本地运行网站
```bash
hexo s
```
**新建文章** 
```bash
hexo n 文章标题 
```
{% asset_img 1624023915496.png %}
会在 source中的_posts中生成一个md文件 

文章头部分 定义标题 图片 描述等等 如
```markdown
---
title: AOP
date: 2021-06-14 09:42:14
author: lovelves
avatar: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1441836571,2166773131&fm=26&gp=0.jpg
authorLink: 
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
tags: springboot
keywords: aop
description: 
photos: https://cdn.jsdelivr.net/gh/lingdas/note/img/6.jpg
---
```
以上这段每个文章必须带上 可以通过修改scaffolds/post_md 来定义每次新增的文章格式

**部署命令**
```bash
hexo d
```
一键部署
```bash
hexo clean && hexo g -d
```
小提示 部署到github前 要安装此插件 否则部署不成功
```bash
cnpm install hexo-deployer-git --save
```
看完以上 建议去官方https://hexo.io/zh-cn/docs/ 看看 加强印象


### 小技巧
##### **用github做图床 访问的时候用这个链接加速**
```txt
https://cdn.jsdelivr.net/gh/github的名字/项目名字/具体图片路径
如下面 我的名字是lingdas 项目是note 具体路径 /img/7.jpg
https://cdn.jsdelivr.net/gh/lingdas/note/img/7.jpg
```
#### **安装看版娘**
```bash
cnpm install --save hexo-helper-live2d
```
模型下载
```bash
cnpm install live2d-widget-model-tsumiki
```
模型列表
```bash
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01
live2d-widget-model-haru/02
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```
模型配置（在根目录_config.yml后面加上这个配置）
```yml
# Live2D
## https://github.com/EYHN/hexo-helper-live2d
live2d:
  enable: true
  # enable: false
  scriptFrom: local # 默认
  pluginRootPath: live2dw/ # 插件在站点上的根目录(相对路径)
  pluginJsPath: lib/ # 脚本文件相对与插件根目录路径
  pluginModelPath: assets/ # 模型文件相对与插件根目录路径
  # scriptFrom: jsdelivr # jsdelivr CDN
  # scriptFrom: unpkg # unpkg CDN
  # scriptFrom: https://cdn.jsdelivr.net/npm/live2d-widget@3.x/lib/L2Dwidget.min.js # 你的自定义 url
  tagMode: false # 标签模式, 是否仅替换 live2d tag标签而非插入到所有页面中
  debug: false # 调试, 是否在控制台输出日志
  model:
    use: live2d-widget-model-haru # 你下载的模型名字
    scale: 1
    hHeadPos: 0.5
    vHeadPos: 0.618
    # use: live2d-widget-model-wanko # npm-module package name
    # use: wanko # 博客根目录/live2d_models/ 下的目录名
    # use: ./wives/wanko # 相对于博客根目录的路径
    # use: https://cdn.jsdelivr.net/npm/live2d-widget-model-wanko@1.0.5/assets/wanko.model.json # 你的自定义 url
  display:
    superSample: 2
    width: 280
    height: 480
    position: right
    hOffset: 0
    vOffset: -20
  mobile:
    show: true # 是否在移动设备上显示
    scale: 0.5 # 移动设备上的缩放       
  react:
    opacityDefault: 0.7
    opacityOnHover: 0.8

```
在我们的Hexo根目录下新建一个live2d_models文件夹，注意：文件夹必须为该名字
把下载的模型复制到这个文件夹中 live2d_models
比如我下载了这个模型  在node_modules 中
{% asset_img 1624025343610.png %}


把上文件复制到刚创建的文件夹中
{% asset_img 1624025471252.png %}
比如 我想用 Bronya这个模型 我就修改配置中的
```markdown
 model:
    use: live2d-widget-model-haru # 这里修改为Bronya
    scale: 1
    hHeadPos: 0.5
    vHeadPos: 0.618
```
#### **加强版看板娘**
如果已经安装过官方提供的live2d，需要先卸载！
```bash
cnpm uninstall hexo-helper-live2d
```
经过张书樵大神魔改后的项目下载地址：https://github.com/stevenjoezhang/live2d-widget
把项目文件下载解压到：themes\主题名字\source\文件夹下
{% asset_img 1624026020645.png %}
打开项目目录进入修改autoload.js文件，将live2d_path设为自己的路径，一般没什么太大变化都为：/live2d-widget/
{% asset_img 1624026103090.png %}
{% asset_img 1624026135012.png %}
然后在 header 模版的header中中引入
{% asset_img 1624026693769.png %}
{% asset_img 1624026744607.png %}

```htmlbars
<!--自定义看板娘-->
  <script src="https://cdn.jsdelivr.net/npm/jquery/dist/jquery.min.js"></script>
  <script src="/live2d-widget/autoload.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/font-awesome/css/font-awesome.min.css"/>
```
这样就可以了看到效果了
{% asset_img 1624026805516.png %}



