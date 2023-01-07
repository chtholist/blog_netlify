# Hugo-ShortCode拓展


- [主题文档 - 扩展 Shortcodes - LoveIt (hugoloveit.com)](https://hugoloveit.com/zh-cn/theme-documentation-extended-shortcodes/#1-style)
- 注意引用图片的格式！！！

{{< image src="/images/first.png" >}}

{{< image src="/images/3.png" >}}

{{< image src="/images/avatar.png" >}}



---

### shotcode  拓展

**Hugo** 提供了多个内置的 Shortcodes, 以方便作者保持 Markdown 内容的整洁.

{{< figure src="/images/3.png" title="Lighthouse (figure)" >}}

#### link

- **href** *[必需]* (**第一个**位置参数)

  链接的目标.

- **content** *[可选]* (**第二个**位置参数)

  链接的内容, 默认值是 **href** 参数的值.

  *支持 Markdown 或者 HTML 格式.*

- **title** *[可选]* (**第三个**位置参数)

  HTML `a` 标签 的 `title` 属性, 当悬停在链接上会显示的提示.

- **rel** *[可选]*

  HTML `a` 标签 的 `rel` 补充属性.

- **class** *[可选]*

  HTML `a` 标签 的 `class` 属性.
  
  {{< link href="https://github.com/upstage/" content=Upstage title="Visit Upstage!" >}}

#### image

- **src** *[必需]* (**第一个**位置参数)

  图片的 URL.

- **alt** *[可选]* (**第二个**位置参数)

  图片无法显示时的替代文本, 默认值是 **src** 参数的值.

  *支持 Markdown 或者 HTML 格式.*

- **caption** *[可选]* (**第三个**位置参数)

  图片标题.

  *支持 Markdown 或者 HTML 格式.*

- **title** *[可选]*

  当悬停在图片上会显示的提示.

- **class** *[可选]*

  HTML `figure` 标签的 `class` 属性.

- **src_s** *[可选]*

  图片缩略图的 URL, 用在画廊模式中, 默认值是 **src** 参数的值.

- **src_l** *[可选]*

  高清图片的 URL, 用在画廊模式中, 默认值是 **src** 参数的值.

- **height** *[可选]*

  图片的 `height` 属性.

- **width** *[可选]*

  图片的 `width` 属性.

- **linked** *[可选]*

  图片是否需要被链接, 默认值是 `true`.

- **rel** *[可选]*

  HTML `a` 标签 的 `rel` 补充属性, 仅在 **linked** 属性设置成 `true` 时有效.

{{< image src="https://hugoloveit.com/images/lighthouse.jpg" caption="Lighthouse (`image`)" src_s="/images/lighthouse-small.jpg" src_l="/images/lighthouse-large.jpg" >}}

#### admonition

支持 **12** 种 帮助你在页面中插入提示的横幅.

note / abstract / info / success / question / warning / failure / danger / bug / example / quote

- **type** *[必需]* (**第一个**位置参数)

  `admonition` 横幅的类型, 默认值是 `note`.

- **title** *[可选]* (**第二个**位置参数)

  `admonition` 横幅的标题, 默认值是 **type** 参数的值.

- **open** *[可选]* (**第三个**位置参数) 

  横幅内容是否默认展开, 默认值是 `true`.

{{< admonition type=tip title="This is a tip" open=false >}}
一个 **技巧tip** 横幅
{{< /admonition >}}

#### mapbox地图

- **lng** *[必需]* (**第一个**位置参数)

  地图初始中心点的经度, 以度为单位.

- **lat** *[必需]* (**第二个**位置参数)

  地图初始中心点的纬度, 以度为单位.

- **zoom** *[可选]* (**第三个**位置参数)

  地图的初始缩放级别, 默认值是 `10`.

- **marked** *[可选]* (**第四个**位置参数)

  是否在地图的初始中心点添加图钉, 默认值是 `true`.

- **light-style** *[可选]* (**第五个**位置参数)

  浅色主题的地图样式, 默认值是[前置参数](https://hugoloveit.com/zh-cn/theme-documentation-content#front-matter)或者[网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration)中设置的值.

- **dark-style** *[可选]* (**第六个**位置参数)

  深色主题的地图样式, 默认值是[前置参数](https://hugoloveit.com/zh-cn/theme-documentation-content#front-matter)或者[网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration)中设置的值.

- **navigation** *[可选]*

  是否添加 [NavigationControl](https://docs.mapbox.com/mapbox-gl-js/api#navigationcontrol), 默认值是[前置参数](https://hugoloveit.com/zh-cn/theme-documentation-content#front-matter)或者[网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration)中设置的值.

- **geolocate** *[可选]*

  是否添加 [GeolocateControl](https://docs.mapbox.com/mapbox-gl-js/api#geolocatecontrol), 默认值是[前置参数](https://hugoloveit.com/zh-cn/theme-documentation-content#front-matter)或者[网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration)中设置的值.

- **scale** *[可选]*

  是否添加 [ScaleControl](https://docs.mapbox.com/mapbox-gl-js/api#scalecontrol), 默认值是[前置参数](https://hugoloveit.com/zh-cn/theme-documentation-content#front-matter)或者[网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration)中设置的值.

- **fullscreen** *[可选]*

  是否添加 [FullscreenControl](https://docs.mapbox.com/mapbox-gl-js/api#fullscreencontrol), 默认值是[前置参数](https://hugoloveit.com/zh-cn/theme-documentation-content#front-matter)或者[网站配置](https://hugoloveit.com/zh-cn/theme-documentation-basics#site-configuration)中设置的值.

- **width** *[可选]*

  地图的宽度, 默认值是 `100%`.

- **height** *[可选]*

  地图的高度, 默认值是 `20rem`.

> 未知错误，无法显示


{{< mapbox lng=118.098312 lat=24.459052 zoom=12 >}}

{{< mapbox lng=-122.252 lat=37.453 zoom=10 marked=false light-style="mapbox://styles/mapbox/streets-zh-v1" >}}


{{< mapbox 121.485 31.233 12 >}}



24.459052, 118.098312

#### 音乐

- **server** *[必需]*

  音乐的链接.

- **type** *[可选]*

  音乐的名称.

- **artist** *[可选]*

  音乐的创作者.

- **cover** *[可选]*

  音乐的封面链接.

- **theme** *[可选]

  音乐播放器的主题色, 默认值是 `#448aff`.

- **fixed** *[可选]*

  是否开启固定模式, 默认值是 `false`.

- **mini** *[可选]*

  是否开启迷你模式, 默认值是 `false`.

- **autoplay** *[可选]*

  是否自动播放音乐, 默认值是 `false`.

- **volume** *[可选]*

  第一次打开播放器时的默认音量, 会被保存在浏览器缓存中, 默认值是 `0.7`.

- **mutex** *[可选]*

  是否自动暂停其它播放器, 默认值是 `true`.

`music` shortcode 还有一些只适用于音乐列表方式的其它命名参数:

- **loop** *[可选]*

  [`all`, `one`, `none`]

  音乐列表的循环模式, 默认值是 `none`.

- **order** *[可选]*

  [`list`, `random`]

  音乐列表的播放顺序, 默认值是 `list`.

- **list-folded** *[可选]*

  初次打开的时候音乐列表是否折叠, 默认值是 `false`.

- **list-max-height** *[可选]*

  音乐列表的最大高度, 默认值是 `340px`.

**歌名中间不能有空格，要用连字符连起来**

##### 本地音乐

{{< music url="/music/01.flac" name=泠鸢 cover="/images/03.png" >}}

##### 音乐平台

- **auto** *[必需]]* (**第一个**位置参数)

  用来自动识别的音乐平台 URL, 支持 `netease`, `tencent` 和 `xiami` 

{{< music auto="https://music.163.com/#/playlist?id=6642034344" >}}

#### bilibili

{{< bilibili id=BV1TJ411C7An p=3 >}} 

#### typeit打字动画

{{< typeit >}}
这一个带有基于 [TypeIt](https://typeitjs.com/) 的 **打字动画** 的 *段落*...
{{< /typeit >}}

{{< typeit group=paragraph >}}
**首先**, 这个段落开始
{{< /typeit >}}

{{< typeit group=paragraph >}}
**然后**, 这个段落开始
{{< /typeit >}}






