# 实战配方（可直接改编）

## 1. 文章页骨架

```
[[div class="meta" style="color:#666; font-size:90%"]]
作者: [[*user alice]] · [[date 1710000000 format="%e %B %Y|agohover"]]
标签: 可用页面底部 tags UI；或文中手写说明
[[/div]]

+ 标题也可在页面 Title 字段设置，正文从简介开始

正文第一段……

++ 小节
* 要点一
* 要点二

[[collapsible show="+ 附录" hide="- 收起附录"]]
长附录……
[[/collapsible]]

[[footnoteblock]]
```

## 2. 信息盒组件（CSI / 本站 include）

`component:infobox` 源码：

```
[[div class="infobox" style="float:right; width:15em; border:1px solid #999; padding:0.5em; margin:0 0 1em 1em;"]]
[[=]]
**{$title}**
[[/=]]
{$body}
[[/div]]
```

调用：

```
[[include component:infobox |
title=物品编号 |
body=**等级:** 待整理_
补充说明一行
]]
```

## 3. 博客索引 + 日历

```
[[module PageCalendar category="blog" urlAttrPrefix="b_"]]

[[module ListPages category="blog" tags="-_hidden" created_at="@URL" order="created_at desc" perPage="10" separate="no" urlAttrPrefix="b_"]]
* **%%title_linked%%** — %%created_by%% · %%created_at|%e %b %Y%%
_ %%preview(160)%%
[[/module]]
```

## 4. ListPages 输出表格

```
[[module ListPages category="staff" order="title" separate="no" wrapper="no"]]
[[head]]
||~ 姓名 ||~ 职务 ||~ 标签 ||
[[/head]]
[[body]]
|| %%title_linked%% || %%form_data{role}%% || %%tags%% ||
[[/body]]
[[/module]]
```

## 5. ListPages + TabView

```
[[module ListPages category="howto" tags="+listpages" order="title" separate="no" prependLine="[[tabview]]" appendLine="[[/tabview]]"]]
[[tab %%title%%]]
%%content%%
[[/tab]]
[[/module]]
```

内容很大时改输出链接，避免一次加载过多。

## 6. 相关页面（同标签）

```
++ 相关
[[module ListPages category="*" tags="=" range="others" limit="8" order="rating desc" separate="no"]]
* %%title_linked%% (%%category%%)
[[/module]]
```

## 7. Data Form 类模板（乐队示例缩略）

`band:_template`：

```
[[module CSS]]
.band-hero img { border: 1px solid #ccc; }
[[/module]]

[[div class="band-hero"]]
[[f<image %%form_raw{bandimage}%% width="160px"]]
[[/div]]

++ %%title%%
* 类型: %%form_data{type}%%
* 网站: %%form_data{bandwebsite}%%
* 录音中: %%form_data{current}%%

%%content%%

====

[[form]]
fields:
  type:
    label: Music type
    type: select
    values:
      0: Classical
      1: Country
      2: Folk
      3: Indie
      4: Jazz
      5: Pop
      6: Rock
    default: 6
  bandimage:
    label: Image
    type: file
  bandwebsite:
    label: Band website
    type: url
  current:
    label: Currently Recording
    type: select
    values:
      0: "Yes"
      1: "No"
    default: 0
[[/form]]
```

列表页：

```
[[module NewPage category="band" button="添加乐队" mode="edit"]]

[[module ListPages category="band" order="title" separate="no"]]
* %%title_linked%% — %%form_data{type}%%
[[/module]]
```

按类型筛选：`_type="6"`（Rock 的 raw 键）。

## 8. 状态切换（iftags + set-tags）

```
[[iftags +draft]]
[[div style="background:#fff3cd; padding:0.5em"]]
此页为草稿
[[/div]]
[[button set-tags -draft +published text="发布"]]
[[/iftags]]

[[iftags +published]]
[[button set-tags -published +draft text="撤回草稿"]]
[[/iftags]]
```

## 9. 隐藏侧栏的单页着陆

```
[[module CSS]]
#side-bar { display: none; width: 0; }
#main-content { margin-left: 0; }
[[/module]]

[[=]]
+ 产品名
一句话说明
[[button edit text="开始编辑本页"]]
[[/=]]
```

（具体选择器随主题 CSS 而变，需对照站点 DOM。）

## 10. Bootstrap 风列表（ListPages head/body/foot）

```
[[module ListPages category="carousel" separate="no" wrapper="no" _active="yes"]]
[[head]]
[[ul id="u-myList" class="list-group"]]
[[/head]]
[[body]]
[[li class="list-group-item"]]%%title_linked%%[[/li]]
[[/body]]
[[foot]]
[[/ul]]
[[/foot]]
[[/module]]
```

需要更紧凑的 HTML 时，外层可用 `[[div_ …]]`（官方明确支持 underscore 吃空白）；`id` 会自动加 `u-` 前缀。

## 11. 抢救坏页的链（写给管理员）

```
[*https://yoursite.wikidot.com/broken-page/norender/true 不渲染打开]
[*https://yoursite.wikidot.com/broken-page/noredirect/true 忽略跳转]
[*https://yoursite.wikidot.com/broken-page#_editpage 直接编辑]
[*https://yoursite.wikidot.com/broken-page#_history 历史]
```

## 12. 简易导航树

侧栏 `nav:side` 片段：

```
[[div class="foldable-list-container"]]
* 文档
 * [[[doc:start|总览]]]
 * [[[doc:wiki-syntax|语法]]]
* 博客
 * [[[blog:start|首页]]]
[[/div]]

+++ 最新
[[module ListPages category="blog" limit="5" separate="no"]]
* %%title_linked%%
[[/module]]
```

块对齐和 div 的写法见 [Wikidot 块格式文档](https://www.wikidot.com/doc-wiki-syntax:block-formatting-elements)。
