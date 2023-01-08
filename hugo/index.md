#  Hugo&Netlify 搭建笔记




## Hugo

[教程1](https://hugo.aiaide.com/)



### 命令

**❗在site主目录下跑命令行**



- `hugo new site [NAME]` —— 创建新的网站

- `hugo new posts/first_post.md` —— 创建新文章

  ⚠注意创建在哪个子目录下
  
- `hugo server`

  跑本地服务器，可以实时更新

  - 重定向到刚修改的页面

    `--navigateToChanged`

- `hugo`

  生成到 `public/` dir ，可以发布了



安装皮肤：

在 `themes` 目录（可能要新建）里把皮肤 `git clone` 下来



发布：

1. `hugo`，所有静态页面都会生成到 `public` 目录
2. 将pubilc目录里所有文件 `push` 到Github



---

Hugo不会发布有此情况的页面（front matter中），可以使用参数更改

| not publish           | 命令行参数       |
| --------------------- | ---------------- |
| 未来发布日期          | `--buildFuture`  |
| draft: true           | `--buildDrafts`  |
| past expirydate value | `--buildExpired` |







### 目录结构

```
example/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── layouts/
├── public/
├── static/
├── themes/
└── config.toml
```



#### archetypes

新建内容的模板
运行`hugo new`时，使用模板/archetypes/default.md

```yaml
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: false
lightgallery: true
author: "Chtholist"
tags: []
categories:[]
featuredImagePreview:""
---
```



- 可以再创建子模板文件夹

  `hugo new --kind [目录名] posts/my-post`

  在/content/posts/my-post里按照以下的模板来新建文件夹

  ```
  archetypes
  ├── default.md
  └── post-bundle
      ├── bio.md
      ├── images
      │   └── featured.jpg
      └── index.md
  ```




#### content

网站的所有内容都在这个目录中



遵循以下目录树

- 注意：markdown只能放在最小子目录中，图片、pdf等每级目录都可放

```
content/
├── about
│   ├── index.md
├── posts
│   ├── my-post
│   │   ├── content1.md
│   │   ├── content2.md
│   │   ├── image1.jpg
│   │   ├── image2.png
│   │   └── index.md
│   └── my-other-post
│       └── index.md
│
└── another-section
    ├── ..
    └── not-a-leaf-bundle
        ├── ..
        └── another-leaf-bundle
            └── index.md
```



- 可以先用命令，在archetypes中创建某个目录的模板





#### assets

- /assets 存储着所有需要用[Hugo Pipes](https://gohugo.io/hugo-pipes/introduction/)渲染的文件，比如图片、音乐
- 只有`.Permalink` or `.RelPermalink` 会被公开发布



#### config

Hugo的配置指令以Json、YAML或TOML存储

每个根设置对象都可以作为自己的文件，并根据环境进行结构化

最简单的设置是，直接放在根目录的`config.toml`  （复杂的设置才用config文件夹）



#### data

store configuration files that can be used by Hugo when generating your website. 

The files must be YAML, JSON, TOML

（一般没用？）



#### layouts

存储生成静态网页`.html`的模板

 Templates include [list pages](https://gohugo.io/templates/list/), your [homepage](https://gohugo.io/templates/homepage/), [taxonomy templates](https://gohugo.io/templates/taxonomy-templates/), [partials](https://gohugo.io/templates/partials/), [single page templates](https://gohugo.io/templates/single-page-templates/), and more.



#### static

存储所有的静态内容： images, CSS, JavaScript, etc. When Hugo builds your site

（一般没用？）







## 后记



2021-1-7

现在回去看之前写的hugo、git等笔记，看的很痛苦。一来当时写笔记时直接用英文写，二来是记了很多东西，很乱，不少知识都是底层的，我一时半会儿碰不到更用不了的。得到的教训就是不要好高骛远，别高估自己，整的那么难那么复杂给自己设门槛，到头来自己都看不懂自己做的笔记，那就违背初衷了，纯粹是为了学而学。做笔记就应该像这Markdown一样，简洁美观且实用。

从下午忙活到晚上，终于搞定了Hugo+Netlify，最耗时的一个是Git，从头配置了一遍，另一个是Hugo和LoveIt，也算是重头来过，把笔记文档重新写了一遍。

