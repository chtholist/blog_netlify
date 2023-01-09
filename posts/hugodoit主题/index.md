#  【Hugo】DoIt主题




### 前言

2021.3.18

第三代个人博客。之前折腾了Gridea，用了好几个主题，魔改移植了live2d/点击特效等一堆花里胡哨的。现在……Hugo+简单的主题，能用就行

本版本blog搭建：

1. 首次使用图床
2. 首次正经使用ladder来看官方文档和debug
3. 2天搭完（虽然原来预期是一晚上）

后续待改进：

1. 手机端用Termux管理博客
2. 电脑端自动部署
3. 完善主题（评论，拓展页面等）



2023.1.7

重写了一遍



2023.1.8

发现LoveIt很久没更，有人fork并做了新主题[DoIt](https://github.com/HEIGE-PCloud/DoIt)，遂迁移



2023.1.9

还是有些bug，遂hugo new site，搞了将近一天



---

---

---





### DoIt主题-基本

{{< admonition type=warning >}}

**注意语法！！！**

1. YAML的空格
2. markdown不会自动补全尖括号`<>`
3. image不要省略成img

{{< /admonition >}}



`hugo serve --disableFastRender` —— 本地实时预览界面


其他：

- [数学公式Katex](https://hugoloveit.com/zh-cn/theme-documentation-content/#%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F)



- 保持博客文章存放在 `content/posts` 目录, 例如: `content/posts/我的第一篇文章.md`
- 保持简单的静态页面存放在 `content` 目录, 例如: `content/about.md`



- 本地资源引用：

1. 将本地资源放在 **assets** 目录中, 默认路径是 `/assets`. 引用资源的文件路径是相对于 assets 目录的.

2. 将本地资源放在 **static** 目录中, 默认路径是 `/static`. 引用资源的文件路径是相对于 static 目录的.

   引用的**优先级**符合以上的顺序.



- 作者配置在`/data/authors/author_name.toml`中，

  在文章前置参数中署名`authors: [name]`



- 友链：

  ```markdown
  {{</* friend "name" "url" "image" "description" */>}}
  ```




- 头像设置：[Gravatar](https://cn.gravatar.com/emails/)，在`config.toml`的主页个人信息[params.home.profile]中



---

---

---



#### 文章前置参数

- markdown文档开头的yaml

{{< admonition type=warning title="注意" >}}

YAML的语法要求极其严格，可以使用[网站](https://www.yamllint.com/)检查语法

- 冒号`:`后面要有空格
- 语句前不能有空格

{{< /admonition >}}



{{< admonition >}}

title: "标题"  
subtitle: "副标题"  
date: 创建时间  
lastmod: 上次修改时间  
draft: 若true，则该文不会被渲染  
authors: "[name]"  
description: "内容描述"  
images: [页面图片]  

tags: [标签]  
categories: [类别]  
series: 系列
series_weight: 在系列的位置
seriesNavigation: 系列导航（T/F）

featuredImage: "文章头图"  
featuredImagePreview: "文章预览图"  

hiddenFromHomePage: 若true，则不会显示在主页  
hiddenFromSearch: 若true，则不会显示在搜索结果  

linkToMarkdown: 页脚指向原始md（T/F）  

{{< /admonition >}}  



#### Katex

[数学公式](https://hugodoit.pages.dev/zh-cn/theme-documentation-content/#%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F)

推荐使用math扩展（不会被解析为html）（DoIt才有）

```markdown
{{</* math */>}}$\|\boldsymbol{x}\|_{0}=\sqrt[0]{\sum_{i} x_{i}^{0}}${{</* /math */>}}
Or
{{</* math */>}}
$$\|\boldsymbol{x}\|_{0}=\sqrt[0]{\sum_{i} x_{i}^{0}}$$
{{</* /math */>}}
```


#### 特殊字符sytle

```markdown
{{</* style "text-align:right; strong{color:#00b1ff;}" */>}}
This is a **right-aligned** paragraph.
{{</* /style */>}}
```

呈现的输出效果如下:

{{< style "text-align:right; strong{color:#00b1ff;}" >}}
This is a **right-aligned** paragraph.
{{< /style >}}

#### 字符注释

```markdown
[Hugo]{?^}(一个开源的静态网站生成工具)
```

[Hugo]^(一个开源的静态网站生成工具)



#### 转义字符


在某些特殊情况下文章内容会与 Markdown 的基本或者扩展语法冲突且无法避免.

转义字符语法可以帮助你渲染出想要的内容:

```markdown
{{??}X} -> X
```

例如, 两个 `:` 会启用 emoji 语法，可以像这样使用转义字符语法:

```markdown
{{??}:}joy:
```

呈现的输出效果如下:

**{?:}joy{?:}** 而不是 **:joy:**

{{< admonition tip >}}
这个方法可以间接解决一个还未解决的 **[Hugo 的 issue](https://github.com/gohugoio/hugo/issues/4978)**.
{{< /admonition >}}

另一个例子是:

```markdown
[link{{??}]}(#escape-character)
```

呈现的输出效果如下:

**[link{?]}(#escape-character)** 而不是 **[link](#escape-character)**.



---

---

---



#### Bug

1. ***failed to extract shortcode: template for shortcode***

   直接原因：官方文档写得有问题，在详细版的config.toml中没有写`theme=DoIt`（好像还有别的错）

   根本原因：I suggest checking the value of `theme` in your config.toml. It should point to the name of the theme that you downloaded to your `themes` folder. If Blogdown installed Academic to `themes/hugo-academic` but the `theme` field in `config.toml` points to just `academic`, then you need to rename your `themes/hugo-academic` folder to `themes/academic` and restart Hugo.

2. ***目录toc***

   只能在开头目录和侧边目录二选一，在themes里的改动都会引发连锁新bug

   - `assets\js\theme.js`中functioninitToc ()
   - `assets\css\_core\_media.scss` ——调节页宽和目录宽
   - `assets\css\_partial\_single\_toc.scss`

3. ***在系列series中写”摘录“会报错***

   提交Issues但被打回了，做了[最小复现](https://antfu.me/posts/why-reproductions-are-required-zh)发现并没有问题，人嘛了说实话。同样的网页语言英文的问题在最小复现中也没出现。

   生无可恋了，这些BUG是怎么产生的呢？就拿语言为英文举例子，config.toml里有三行是语言配置，出问题的和最小复现都一样啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊

   最后发现，config.toml中的主题版本号`version = "0.2.X"`，放在语言的配置之后就正常了。

   series的问题我是真的没辙了，把原来site的theme和config.toml复制过去都没有复现问题，我真不知道这bug哪来的

> 配置文件中语句的顺序会导致bug我是真的没想到。一来toml我没学过，二来是我默认配置文件是无关语句顺序的。这算是定式思维吗？不知道，不过我编程思维不足是肯定的，但这一时半会儿没法找补啊



###  拓展shotcode

Shortcodes可以保持 Markdown 内容的整洁.

> 注释的时写`/*`和`*/`

- [自定义字体样式](https://hugoloveit.com/zh-cn/theme-documentation-extended-shortcodes/#1-style)



 #### 图片
  最好用html格式：

  `{{</* image src="xxx.jpg"  alt="无法显示时的替代文本" caption="标题" title="点击提示" height="高度" width="宽度"*/>}}`



#### 网页块

```markdown
{{</*  showcase title="名称" summary="介绍" image="图片" link="链接"   */>}}
```



#### 段落块

```markdown
{{</* admonition type=[类型] title="标题" open=true（默认展开 */>}}
内容
{{</* /admonition */>}}
```

{{< admonition >}}
一个 **注意 [无]** 横幅
{{< /admonition >}}

{{< admonition info >}}
一个 **信息 [info]** 横幅
{{< /admonition >}}

{{< admonition success >}}
一个 **成功 [success]** 横幅
{{< /admonition >}}

{{< admonition warning >}}
一个 **警告 [warning]** 横幅
{{< /admonition >}}

{{< admonition failure >}}
一个 **失败 [failure]** 横幅
{{< /admonition >}}

{{< admonition quote >}}
一个 **引用 [quote]** 横幅
{{< /admonition >}}



---

---




#### 音乐



##### 本地音乐

- root为assets文件夹

```markdwon
{{</* music url="/music/01.flac" name=歌名（不能有空格） artist=歌手 cover="images\avatar.png" */>}}
```

其他参数

- `mini=false` 迷你模式（只有图标）
- `autoplay=false`
- `mutex=true`  自动暂停其他播放器
- `loop=none` 循环播放，参数：all / one / none
- `order=list` 播放顺序，参数：list / random
- `list-folded=false` 折叠列表

{{< music url="/music/01.flac" name=折纸信笺 artist=泠鸢 cover="images\avatar.png" >}}



##### 音乐平台

- **server** *[必需]* (**第一个**位置参数)

  [`netease`, `tencent`, `kugou`, `xiami`, `baidu`]

  音乐平台.

- **type** *[必需]* (**第二个**位置参数)

  [`song`, `playlist`, `album`, `search`, `artist`]

  音乐类型.

- **id** *[必需]* (**第三个**位置参数)

  歌曲 ID, 或者播放列表 ID, 或者专辑 ID, 或者搜索关键词, 或者创作者 ID.



{{< music auto="https://music.163.com/#/playlist?id=6642034344" >}}



#### 视频

Youtube（b站同理）

`  {{</* youtube co59abJjokc */>}} `

{{< youtube co59abJjokc >}}



#### 打字动画


```markdown
{{</* typeit code=java */>}}
public class HelloWorld {
    public static void main(String []args) {
        System.out.println("Hello World");
    }
}
{{</* /typeit */>}}
```

呈现的输出效果如下:

{{< typeit code=java >}}
public class HelloWorld {
    public static void main(String []args) {
        System.out.println("Hello World");
    }
}
{{< /typeit >}}


