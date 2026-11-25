# 常用模块速查

通用语法：

```
[[module ModuleName param="value" …]]
可选体
[[/module]]
```

完整目录：https://www.wikidot.com/doc-modules:start

## CSS

见 [layout-advanced](layout-advanced.md)。按页/组件注入样式；CSI 可携带样式包。

## Rate / RatedPages

需在 Site Manager 启用分类评分。

```
[[module Rate]]
```

五星模式下模块体可用：`%%rating%%` `%%rating_votes%%` `%%rating_percent%%` `%%rating_decimal%%`。

```
[[=]]
[[module Rate]]
平均 %%rating_decimal%%（%%rating_votes%% 票）
[[/module]]
[[/=]]
```

`[[module RatedPages]]`：展示高分页面列表（参数见官方页）。

## Comments

```
[[module Comments]]
[[module Comments hideForm="true"]]
```

常放在 `category:_template` 的 `%%content%%` 下方，使整类页面自动带评论。

## NewPage

```
[[module NewPage category="doc" size="20" button="新建" tags="draft" parent="docs:hub" template="template:doc" format="/^[a-z0-9-]+$/"]]
```

- `template` 必须指向已存在的 `template:…` 页（可逗号多选）
- `mode`：`edit`（默认进编辑器）/ `save-and-refresh` / `save-and-go`；`goTo` 指定保存后跳转
- **不能**用 NewPage 创建 `_` 隐藏页（含 `_template`）——需浏览器地址栏直接打开 `category:_template`

## Redirect

```
[[module Redirect destination="start"]]
[[module Redirect destination="http://example.com/base/"]]
```

- 纯页名 → 站内跳转
- 完整 URL → 外链
- destination 以 `/` 结尾 → 路径映射（把当前页后续 path 接到目标后）
- 编辑页时加 `/noredirect/true` 避免无法编辑

## CountPages

选择器与 ListPages 同类子集；体中输出 `%%total%%`：

```
[[module CountPages category="wiki,blog" tags="+open -_closed"]]
共 %%total%% 篇。
[[/module]]
```

## PageCalendar

按年/月统计发文，常与 ListPages 的 `created_at="@URL"` 联动；两边 `urlAttrPrefix` 保持一致。

```
[[module PageCalendar category="blog"]]
[[module ListPages category="blog" created_at="@URL" perPage="10" separate="no"]]
* %%title_linked%% — %%created_at|%e %b %Y%%
[[/module]]
```

## TagCloud / PageTree / Backlinks / Files

```
[[module TagCloud]]
[[module PageTree]]
[[module Backlinks]]
[[module Files]]
```

用于标签云、父子树、反向链接、附件列表。具体参数见各模块文档页。

## ListUsers / Members / Join / Watchers

用户与成员相关；权限敏感，按站点政策使用。

## Feed / FrontForum

- `Feed`：拉取外部 RSS/Atom 并格式化显示  
- `FrontForum`：把论坛分区当新闻源 + RSS  

## Search / SearchAll / SearchUsers / SiteChanges

站内/全站搜索与最近更改列表。

## Gallery / FlickrGallery

模块版图库（与语法 `[[gallery]]` 互补）。Flickr 拉取见 FlickrGallery 文档。

## MailForm / PetitionAdmin

邮件表单与请愿；注意隐私与垃圾信息。

## ThemePreviewer / Clone / SiteGrid / FeaturedSite

主题预览、站点克隆、站点橱窗——偏站务/门户。

## 已弃用（遇到旧站源码时）

`Pages`、`ChildPages`、`PagesByTag`、`NextPage`、`PreviousPage`、`MembershipByPassword`、`MembershipApply`、`SimpleToDo`、`AdSenseUnit`、`ManageSite`（作模块）等 → 优先改写为 **ListPages** / 现代成员流程。

## 模块组合禁忌（经验法则）

1. ListPages / CountPages 体内再塞 Comments、ListPages、Rate 等 —— 通常失败或不稳定  
2. 需要“列表 + 详情组件”时：ListPages 只输出链接/摘要；详情组件放在被链到的页面或 `_template`  
3. 同页多个会读 URL 的模块 → 设不同 `urlAttrPrefix`  
4. 抢救含坏模块的页：访问 `/norender/true` 或 `#_editpage` / 历史回滚
