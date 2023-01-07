#  Hugo-博客搭建


## Hugo——博客搭建

### 前言

第三代个人博客。之前折腾了Gridea，用了好几个主题，魔改移植了live2d/点击特效等一堆花里胡哨的。现在……Hugo+简单的主题，能用就行

本版本blog搭建：

1. 首次使用图床
2. 首次正经使用ladder来看官方文档和debug
3. 2天搭完（虽然原来预期是一晚上）

后续待改进：

1. 手机端用Termux管理博客
2. 电脑端自动部署
3. 完善主题（评论，拓展页面等）



### Hugo 基本操作

在hugo博客的文件夹底下打开命令行窗口

1. ```
   hugo serve
   ```

   构建本地运行环境，在`http://localhost:1313/`显示，可以实时更新，用来检查bug和预览最终效果
   
2. ```
   hugo
   ```

   生成一个 public 目录, 目录内包含了网站的所有静态内容和资源。在`public`文件夹中



> https://blog.csdn.net/qq_34688164/article/details/104615706



### LoveIt主题-基本

3. 由于本主题使用了 Hugo 中的 `.Scratch` 来实现一些特性, 非常建议你为 `hugo server` 命令添加 `--disableFastRender` 参数来实时预览你正在编辑的文章页面.

4. `hugo serve` 的默认运行环境是 `development`, 而 `hugo` 的默认运行环境是 `production`.

   由于本地 `development` 环境的限制, **评论系统**, **CDN** 和 **fingerprint** 不会在 `development` 环境下启用.

   可以使用 `hugo serve -e production` 命令来开启这些特性.

5. 默认的 CDN 数据文件位于 `themes/LoveIt/assets/data/cdn/` 目录. 可以在你的项目下相同路径存放数据文件: `assets/data/cdn/`.

##### 构建网站

`hugo`  ——会生成一个 `public` 目录, 其中包含你网站的所有静态内容和资源. 现在可以将其部署在任何 Web 服务器上.

##### 内部搜索

以下是两种搜索引擎的对比:

- `lunr`: 简单, 无需同步 `index.json`, 没有 `contentLength` 的限制, 但占用带宽大且性能低 (特别是中文需要一个较大的分词依赖库)
- `algolia`: 高性能并且占用带宽低, 但需要同步 `index.json` 且有 `contentLength` 的限制

文章内容被 `h2` 和 `h3` HTML 标签切分来提高查询效果并且基本实现全文搜索. `contentLength` 用来限制 `h2` 和 `h3` HTML 标签开头的内容部分的最大长度.

##### 引用图片和音乐等本地资源

1. 使用[页面包](https://gohugo.io/content-management/page-bundles/)中的[页面资源](https://gohugo.io/content-management/page-resources/). 你可以使用适用于 `Resources.GetMatch` 的值或者直接使用相对于当前页面目录的文件路径来引用页面资源.
2. 将本地资源放在 **assets** 目录中, 默认路径是 `/assets`. 引用资源的文件路径是相对于 assets 目录的.
3. 将本地资源放在 **static** 目录中, 默认路径是 `/static`. 引用资源的文件路径是相对于 static 目录的.

引用的**优先级**符合以上的顺序.

在这个主题中的很多地方可以使用上面的本地资源引用, 例如 **链接**, **图片**, `image` shortcode, `music` shortcode 和**前置参数**中的部分参数.

页面资源或者 **assets** 目录中的[图片处理](https://gohugo.io/content-management/image-processing/)会在未来的版本中得到支持. 非常酷的功能! 

### 前置参数

**Hugo** 允许你在文章内容前面添加 `yaml`, `toml` 或者 `json` 格式的前置参数.

- **title**: 文章标题.
- **subtitle**: 文章副标题.
- **date**: 这篇文章创建的日期时间. 它通常是从文章的前置参数中的 `date` 字段获取的, 但是也可以在 [网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration) 中设置.
- **lastmod**: 上次修改内容的日期时间.
- **draft**: 如果设为 `true`, 除非 `hugo` 命令使用了 `--buildDrafts`/`-D` 参数, 这篇文章不会被渲染.
- **author**: 文章作者.
- **authorLink**: 文章作者的链接.
- **description**: 文章内容的描述.
- **license**: 这篇文章特殊的许可.
- **images**: 页面图片, 用于 Open Graph 和 Twitter Cards.
- **tags**: 文章的标签.
- **categories**: 文章所属的类别.
- **featuredImage**: 文章的特色图片.
- **featuredImagePreview**: 用在主页预览的文章特色图片.
- **hiddenFromHomePage**: 如果设为 `true`, 这篇文章将不会显示在主页上.
- **twemoji**:如果设为 `true`, 这篇文章会使用 twemoji.
- **lightgallery**: 如果设为 `true`, 文章中的图片将可以按照画廊形式呈现.
- **ruby**: 如果设为 `true`, 这篇文章会使用 [上标注释扩展语法](https://hugoloveit.com/zh-cn/theme-documentation-content/#ruby).
- **fraction**: 如果设为 `true`, 这篇文章会使用 [分数扩展语法](https://hugoloveit.com/zh-cn/theme-documentation-content/#fraction).
- **fontawesome**: 如果设为 `true`, 这篇文章会使用 [Font Awesome 扩展语法](https://hugoloveit.com/zh-cn/theme-documentation-content/#fontawesome).
- **linkToMarkdown**: 如果设为 `true`, 内容的页脚将显示指向原始 Markdown 文件的链接.

```yaml
---
title: "我的第一篇文章"
subtitle: ""
date: 2020-03-04T15:58:26+08:00
lastmod: 2020-03-04T15:58:26+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: []
categories: []
featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: true
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  # ...
math:
  enable: true
  # ...
mapbox:
  accessToken: ""
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # 位于 "assets/"
    # 或者
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # 位于 "assets/"
    # 或者
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---

```


