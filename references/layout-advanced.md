# 进阶布局：对齐、div/span、TabView、CSS

页面外壳和渲染后的组件结构见[页面与组件结构](page-layout.md)。

## 块对齐

标签必须独占一行，行尾无空格：

```
[[<]]
左对齐
[[/<]]

[[>]]
右对齐
[[/>]]

[[=]]
居中
[[/=]]

[[==]]
两端对齐
[[/==]]

```

单行居中：行首 `= 文字`。

段落内续行可用行尾 `\`。

清浮动：单独一行 `~~~~`（约等于 clearfix）。

## `[[div]]` / `[[span]]`

允许属性：`id`、`class`、`style`、`data-*`（以文档为准；span 通常无 id 需求时用 class/style/data）。

```
[[div class="row" style="display:flex; gap:1em;"]]
[[div class="col" style="flex:1"]]
左栏
[[/div]]
[[div class="col" style="flex:1"]]
右栏
[[/div]]
[[/div]]
~~~~
```

规则：
- 开闭标签各自独占一行，可嵌套
- 自定义 `id="myCarousel"` → 输出 `id="u-myCarousel"`；若已写 `u-` 则不再加
- **`[[div_ class="…"]]`**：吃掉标签周围空白，避免多余 `<p>`/换行——拼 Bootstrap 组件时几乎必备
- 同行还有 `[[a_ …]]` 等同理

两栏 float 经典写法：

```
[[div style="float:left; width:45%; padding:0 2%"]]
左
[[/div]]
[[div style="float:left; width:45%; padding:0 2%"]]
右
[[/div]]
~~~~
```

## TabView

```
[[tabview]]
[[tab 标题一]]
内容一
[[/tab]]
[[tab 标题二]]
内容二
[[/tab]]
[[/tabview]]
```

限制（设计时必须考虑）：
- **不能嵌套** tabview
- TOC / 页内锚点 / 浏览器“后退”在 tab 内不可靠
- 适合短切换面板，不适合把整篇长文塞进多 tab

与 ListPages 组合：见 [examples](examples.md) 与 [listpages](listpages.md)（`separate="no"` + `prependLine="[[tabview]]"` + `appendLine="[[/tabview]]"`，body 内 `[[tab %%title%%]]…[[/tab]]`）。

## 页面级 CSS

```
[[module CSS]]
#side-bar { display: none; }
.page-rate-widget-box { margin: 0.5em 0; }
[[/module]]
```

参数：
- `show="true"` — 同时把 CSS 以代码块形式显示在页上
- `disable="true"` — 保留源码但不生效

多条 CSS 模块按出现顺序注入 `<head>`。CSI include 进来的 CSS 模块会随 include 作用到宿主页——组件化样式常用此法。

主题级 CSS 在 Site Manager / 主题页配置；模块 CSS 适合单页或组件包。

## HTML 块 vs wiki 布局

| 手段 | 用途 |
|------|------|
| `[[div]]`/`[[span]]` + CSS | 首选：仍是 wiki 流，可含模块/链接 |
| `[[html]]…[[/html]]` | 隔离 iframe 中的任意 HTML；**内部 wiki 不解析** |
| `[[iframe url …]]` | 嵌入外部页；属性：`width`/`height`/`scrolling`/`frameborder`/`align`/`class`/`style` |
| `[[embed]]` / `[[embedvideo]]` / `[[embedaudio]]` | 粘贴白名单服务的嵌入代码 |

能用 div+CSS 就不要上 html iframe，以便与正文共享样式和布局。

## iftags（按标签显隐）

```
[[iftags +science]]
仅当页有 science 标签时显示
[[/iftags]]

[[iftags +bug -fixed]]
有 bug 且没有 fixed
[[/iftags]]
```

- `+tag` 或裸 `tag`：必须有
- `-tag`：必须没有
- 隐藏标签 `_foo` 同样可用于条件
- 常配合 `[[button set-tags …]]` 做状态切换 UI

在 ListPages 中：`separate="yes"` 时 iftags 看**每一项页面**的标签；`separate="no"` 时看**宿主页**标签。

## 侧栏 / 导航惯例

常见隐藏/系统页（站点可变）：
- `nav:side`、`nav:top` — 导航
- `admin:manage` — 站点管理（权限）
- `_template`、`category:_template` — live template
- `_404`、`category:_404` — 不存在页提示

侧栏里用列表 + include + ListPages 做动态导航是常规进阶手法。

块对齐和 div 的写法见 [Wikidot 块格式文档](https://www.wikidot.com/doc-wiki-syntax:block-formatting-elements)。
