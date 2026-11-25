# Wikidot 通用页面与组件结构

这里说明页面区域、CSS 作用域和组件交互。涉及具体站点的尺寸、字体或侧栏定位时，参阅[后室中文阈限](layout-backrooms-cn.md)或[SCP 中文 Sigma-9](layout-scp-cn.md)，并对照目标页的实际主题。

## 页面、主题与组件的关系

- **Wikidot 平台**编译 wiki 源码，提供页面外壳、导航区域、标签、历史、附件与编辑功能。
- **站点基础主题**定义外壳和常见正文元素的布局、颜色、字体、间距与响应式规则。
- **页面主题**通过页面 CSS 或 include 覆盖基础主题。首页与普通文章可以使用不同的覆盖规则。
- **组件**由结构、样式和必要的交互组成。评分栏、图片框、脚注、折叠、分页不一定采用相同的实现方式。

Wikidot、Sigma-9、Black Highlighter、阈限不是同义词。“基础版式”可以指平台基础样式，也可以指站点默认主题。相同 DOM 结构可以被不同主题绘制成不同外观；相似外观也不能证明主题继承关系。

## 页面外壳与正文作用域

以下是常见页面结构的简化层级，不是 Wikidot 编辑器源码。页面可以增加包装层、隐藏标题或省略部分节点。

```
#container-wrap-wrap
  #container-wrap
    #container
      #header
        h1 / h2                 站名与站点副标题
        #search-top-box         搜索区域
        #top-bar                顶部导航
      #content-wrap
        #side-bar               侧栏
        #main-content
          #action-area-top      顶部操作区
          #page-title           当前页标题
          #page-content         编译后的正文与正文组件
          .page-tags            页面标签
          #page-info-break      页面信息分隔位置
          #page-options-container  页面操作入口
          #action-area          动态操作区
      #footer                   平台页脚
      #license-area             授权说明
```

| 结构 | 版式含义 |
|---|---|
| `#header h1` / `#header h2` | 站点标题与副标题，不是文章标题 |
| `#page-title` | 当前页面标题，通常是 div；可以隐藏或不存在 |
| `#page-content h1` 至 `h6` | 正文各级标题；不包含站名和页面标题 |
| `#main-content` | 包含正文、标题、标签与操作区；对它设置布局会影响这些区域 |
| `#page-content` | 正文作用域；对其后代设置样式仍会影响嵌入正文的组件 |
| `#side-bar` 与 `#main-content` | 内容区中的相邻区域，侧栏宽度、主内容边距与外层布局共同决定正文位置 |
| `#action-area` | 页面功能动态加载的位置，不属于正文；评分面板等可以在这里出现 |
| `#login-status` | 账户区域；视觉上位于右上角不意味着 DOM 上属于 `#header` |

顶栏与侧栏通常使用 `nav:top`、`nav:side` 等配置页面提供内容，并非每篇文章都在正文中复制一套导航。`#footer`、`#license-area` 与正文内的著作信息组件分别属于平台页脚、授权说明和作品署名。

单独隐藏 `#side-bar` 不会自动消除主内容的左边距或外层 Grid/Flex 留出的空间。`display:none`、`visibility:hidden` 和 `transform` 也不等价：前者移除布局盒，第二种保留布局空间，第三种改变视觉位置而不按新位置重新排版。

## CSS 层叠与组件边界

- 平台基础样式、站点主题、页面 CSS、include 带入的 CSS、内联声明共同参与层叠。结果由重要性、来源、选择器优先级与顺序等决定，不是最后一张样式表无条件覆盖全部。
- `[[module CSS]]` 和 include 中的 CSS 作用于宿主页，不会因为源码写在某个 div 内就自动局限在该 div。组件根类下的后代选择器与全局 `p`、`a`、`img`、`h1` 的影响范围不同。
- `.yui-navset .yui-nav a` 会匹配作用域内所有分页标题链接；限定到某个组件根类后，才只影响该组件内部的分页。
- `#page-content .scp-image-caption` 针对正文图片框图注；对所有 `p` 设置间距或字体则可能同时影响正文、图注、评分信息与折叠内容。
- `::before`、`::after` 可以绘制标志、按钮提示或替代文字。伪元素内容、背景图与真实 DOM 文本是不同来源。
- `[[html]]` 创建独立 iframe 文档，内部不自动继承父页 CSS，也不解析内部的 Wikidot 语法。普通 div 与 iframe 的样式边界不同；轮播 iframe 中的图片也不属于父文档的普通图片节点。
- 主题提供的样式类不属于 Wikidot 语法关键字；类名存在不代表当前主题已经为它定义样式。各站主题类见对应站点子文档。

## 评分栏与著作信息

标准评分栏的结构为：

```
div.page-rate-widget-box
  span.rate-points
    span.number.prw<动态编号>
  span.rateup.btn.btn-default > a
  span.ratedown.btn.btn-default > a
  span.cancel.btn.btn-default > a
```

| 选择器 | 作用 |
|---|---|
| `.rate-points` | 分数标签与数值区域 |
| `.rate-points .number` | 带正负号的评分数字 |
| `.rateup a` | 正票操作 |
| `.ratedown a` | 负票操作 |
| `.cancel a` | 撤销自己的投票；不是关闭组件 |

操作链接可以使用 `href="javascript:;"`，由 `WIKIDOT.modules.PageRateWidgetModule` 的事件处理函数执行投票。数字样式、按钮样式和实际投票功能是不同层；复制一段渲染后的 HTML 不等于创建了已关联当前页面的评分模块。评分模块语法见[常用模块](modules.md)。

`.page-rate-widget-box` 也可能被其他组件复用。例如 `.translations.page-rate-widget-box` 可以承载语言切换链接，不含评分数字。同一页面的正文和著作信息面板还可以各显示一份相同评分。外框数量不等于评分模块数量，重复显示的数字不能相加；动态 `prw…` 类不适合作为固定的主题选择器或页面 ID。

正文中的评分框与页面工具中的评分面板是两个位置。`#page-options-container #pagerate-button` 是页底评分入口，打开的评分面板加载到 `#action-area`；面板内仍可使用相同的 `.page-rate-widget-box`。只限定 `#page-content` 的样式不会自动覆盖这个动态操作区，正文不显示评分栏也不意味着页面没有评分。

### 可选的著作信息组件（非平台标准结构）

下面是特定著作信息组件可使用的结构，不是标准 Rate 模块自带结构，也不保证两站或每篇文章都加载。仅在页面实际使用相应组件时解释这些类：

```
ul.creditRate
  li.rateBox.folded / li.rateBox.unfolded
    div.rate-box-with-credit-button
      div.page-rate-widget-box
      div.creditButton.foldable-list-container > p > a
    div#u-credit-view
      div.fader.foldable-list-container > a
      div.modalcontainer
        div.modalbox
          div.close-credits.foldable-list-container > a
          div.credit.first
```

`folded` 与 `unfolded` 表示信息面板状态；`.creditButton` 是信息入口，`.close-credits` 是关闭入口，`.fader` 是背景遮罩，`.credit.first` 承载著作信息。它们与评分栏中的 `.cancel` 不是同一操作。

信息覆盖层可以使用 fixed 定位、内部独立滚动和 `visibility` 切换。`display:flex` 不代表面板可见；`visibility:hidden` 仍可隐藏它，过渡动画期间的状态也不等于稳定展开状态。著作信息组件的署名、致谢、图片授权等内容分别有自己的含义。

## 图片框与图注

两站部分页面会使用 `.scp-image-block`，这是共享的组件类名，不属于平台必有结构，也不能用类名中的 scp 判断站点或主题。采用此组件时，常见结构为：

```
div.scp-image-block.block-right
  img.image
  div.scp-image-caption
    p
```

图片可以额外包在链接中。图片、图片框与图注是三个不同的样式对象：外框负责浮动、宽度、边距、边框与阴影；图片负责自身显示尺寸；图注负责说明文字的排版。头像、国旗、无图注的普通图片和轮播 iframe 不一定使用这种结构。

- `.block-right` 可使整个图片框右浮动，后续正文在左侧环绕；浮动对象是外框，不是把图注单独移到图片旁边。
- `width`、`max-width`、媒体查询、内边距与边框共同决定最终外尺寸。内容宽 300px、左右各 1px 边框的 content-box 外框，其边界矩形宽度为 302px。
- 相同内联 `width:300px` 在窄屏下可以被 `max-width` 约束为更窄的图框，也可以仍占据接近整行。近似居中的视觉位置并不必然表示 `float:none`。
- 图注 `.scp-image-caption` 与其图片属于同一外框；图注的居中、字号、内边距与边缘效果由当前主题和组件设置；两站的样式差异分别见站点子文档，不能由相同 DOM 推定。
- 图像的自然尺寸与渲染尺寸不同。`naturalWidth`/`naturalHeight` 表示解码尺寸，CSS 宽高表示布局尺寸；固定一边并保持另一边自动计算可以维持原始比例。
- `getAttribute('src')` 是原始属性，`currentSrc` 是浏览器选用的资源地址，两者可能因自动升级 HTTPS 等机制不同。`complete=true` 也可能出现在加载失败后，不能单独代表图片可用。
- `displ{$captionless}y:none;` 这种未展开参数形成的属性名不是合法的 `display` 声明。浏览器会忽略无效声明，不能凭字符串中的 `none` 判断图注被隐藏。

include 中的相对图片地址按宿主页上下文解析；组件自身附件与宿主页附件不是同一个路径。组件参数及图片路径规则见[模板与表单](templates-forms.md)，图片语法见[基础语法](basics.md)。

## 脚注

标准脚注包含引用端与注释端：

```
正文 p …
  sup.footnoteref
    a.footnoteref#footnoteref-N

div.footnotes-footer
  div.title
  div.footnote-footer#footnote-N
    a
    … 注释正文
```

| 结构 | 含义 |
|---|---|
| `sup.footnoteref` | 正文中的上标容器 |
| `a.footnoteref#footnoteref-N` | 第 N 个脚注引用与交互入口 |
| `.footnotes-footer` | 脚注区容器 |
| `.footnote-footer#footnote-N` | 对应注释正文 |
| `.footnote-footer > a` | 序号与返回引用的入口 |
| `.hovertip` | 悬停提示容器；也用于非脚注的帮助提示 |

`sup` 与内部 `a` 同时具有 `.footnoteref`，因此用这个类计数会把一个引用算成两个节点；定位引用链接可用 `a.footnoteref[id^="footnoteref-"]`，注释正文可用 `.footnote-footer[id^="footnote-"]`。

引用与注释按编号 N 关联。链接可以使用 `javascript:;`，分别由 `scrollToReference('footnote-N')` 和 `scrollToReference('footnoteref-N')` 处理，而非直接使用带 hash 的 href。

脚注引用可以位于折叠内容或非活动分页中，注释区则位于外部。引用被祖先隐藏不破坏其编号关系。悬停提示中的注释是显示副本，不是新增脚注；它的样式作用域也不必与正文中的引用位置相同。

脚注内容可能带强调、链接和其他行内结构，不能只把序号之后的一段纯文本当成所有脚注的固定形式。脚注源码语法与脚注区位置见[基础语法](basics.md)。

## 标准折叠

标准 `[[collapsible]]` 的渲染结构为：

```
div.collapsible-block
  div.collapsible-block-folded
    a.collapsible-block-link
  div.collapsible-block-unfolded
    div.collapsible-block-unfolded-link
      a.collapsible-block-link
    div.collapsible-block-content
```

| 状态 | `.collapsible-block-folded` | `.collapsible-block-unfolded` | 内容区 |
|---|---|---|---|
| 收起 | `display:block` | `display:none` | 已在 DOM 中，因祖先隐藏而不可见 |
| 展开 | `display:none` | `display:block` | 原内容区变为可见 |

展开入口与收起入口都使用 `.collapsible-block-link`。它们位于不同的包装层中，不是同一按钮通过文字替换产生的两个状态。正文位于 `.collapsible-block-content`，其中可以继续包含段落、图片、脚注等结构。

在单个折叠容器中，直接子级关系如下：

- 展开入口：`:scope > .collapsible-block-folded > a`。
- 收起入口：`:scope > .collapsible-block-unfolded > .collapsible-block-unfolded-link > a`。
- 正文：`:scope > .collapsible-block-unfolded > .collapsible-block-content`。

宽泛的 `.collapsible-block-link` 规则会同时影响两种入口，也可能影响嵌套组件。内容自身的 `display:block` 不抵消外层的 `display:none`；折叠外还有隐藏分页时，展开内部折叠也不使整段内容出现在当前页面。

正文附录、实体清单、版式代码、授权信息都可以使用相同折叠结构，内容用途不由类名决定。colmod、`details/summary` 与自定义 CSS 折叠是其他实现，不能套用标准折叠的全部层级和状态规则。折叠语法见[基础语法](basics.md#引用与折叠)。

## 分页（TabView）

Wikidot 的标准 TabView 使用 YUI 容器：

```
div.yui-navset.yui-navset-top#wiki-tabview-<动态标识>
  ul.yui-nav
    li.selected > a > em
    li > a > em
  div.yui-content
    div#wiki-tab-0-0
    div#wiki-tab-0-1
    …
```

| 选择器 | 样式与交互含义 |
|---|---|
| `.yui-navset` | 一组分页的根容器 |
| `.yui-nav` | 标题列表 |
| `.yui-nav > li.selected` | 当前活动标题 |
| `.yui-nav > li > a > em` | 标题链接和文字包装；em 不意味着最终必须显示斜体 |
| `.yui-content` | 面板容器 |
| `.yui-content > div` | 与同组标题顺序对应的直接面板 |

活动面板为 `display:block`，其他面板为 `display:none`。切换标题会改变同组的 selected 状态与面板可见性，未活动面板的内容仍保留在 DOM 中。链接可以使用 `javascript:;`，不需要 `<button>` 或 `aria-selected` 才能组成该组件。

一个页面可以有多组分页。不同组可能重复生成 `wiki-tab-0-0`、`wiki-tab-0-1` 等面板 ID，标题文字也可能重名。因此全局 `#wiki-tab-0-1` 或 `getElementById` 不能稳定表示“所需那一组的第二页”。组内关系由选定的 `.yui-navset` 下 `:scope > .yui-nav > li` 与 `:scope > .yui-content > div` 的顺序决定；动态 `wiki-tabview-…` 标识不适合作为永久固定选择器。

对 `.yui-nav a` 的样式会影响标题按钮，对 `.yui-nav .selected a` 的样式影响活动标题，对 `.yui-content` 的样式影响内容容器；三者的背景、边框、间距与显示状态互不等同。全局覆写面板的 `display` 可能使多个面板同时显示，并破坏原有切换效果。

非活动面板中的图片、脚注、折叠仍属于该面板。分页用于文章的不同版本或不同部分时，隐藏内容不等于被删除，也不应把各版本内容混成同一段正文。

`.footer-wikiwalk-nav` 中的上一篇/下一篇链接是跨文章导航；ListPages 的列表分页是另一种分页；二者都不等于 TabView。TabView 的源码、嵌套限制与 ListPages 组合方式见[进阶布局](layout-advanced.md)和[ListPages](listpages.md)。
