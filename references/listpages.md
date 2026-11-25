# ListPages 深度指南

ListPages 是 Wikidot 最重要的进阶模块：按条件选页、排序、分页、用 `%%变量%%` 模板化输出，并可导出 RSS。

```
[[module ListPages 参数…]]
模块体（每条匹配页套用一次，除非另有 head/body/foot）
[[/module]]
```

默认：当前 category 的可见页，按创建时间降序。  
**模块体内禁止出现 `[[code]]` 或 `[[html]]`**，否则整模块失效。  
多数情况下 **不要在 ListPages 体内再嵌套其他 module**。

官方：https://www.wikidot.com/doc-modules:listpages-module

## 选择器（可组合，取交集）

| 参数 | 要点 |
|------|------|
| `pagetype` | `normal`（默认，非 `_` 页）/ `hidden` / `*` |
| `category` | `.` 当前；`*` 全部；多 category 空格/逗号；`-cat` 排除；在 `_template` 里可用 `%%category%%` |
| `tags` | 默认 OR；`+tag` AND 必须有；`-tag` 必须无；`-` 无任何标签；`=` 与当前页可见标签有交集；`==` 可见标签完全相同 |
| `parent` | `-` 无父；`=` 同父（兄弟）；`-=` 不同父；`.` 当前页的子页；或完整页名 |
| `link_to` | 链向某页的页面；`.` = 链向当前页 |
| `created_at` / `updated_at` | `yyyy` / `yyyy.mm`；比较符 `> < = <= >= <>`；`last 7 day`；`older than 1 month`（UTC） |
| `created_by` | 用户名；`=` 当前页作者；`-=` 非当前作者 |
| `rating` / `votes` | 数字或 `=`；可加比较符。跨不同评分模式 category 时不可靠 |
| `offset` | 跳过前 n 条 |
| `range` | `.` 当前；`before` / `after` / `others` |
| `name` | 不含 category 的名；`=` 同名；`s%`/`s*` 前缀 |
| `fullname` | 完整名精确匹配 |
| `_字段` | Data Form：`_gender="m"` |

## 排序 `order`

格式：`属性` 或 `属性 desc`（再写 `desc desc` 可抵消回升序）。  
默认：`created_at desc`。  
**不要写 `property asc`**（非法，会回退默认）。

常用属性：`name` `fullname` `title` `created_by` `created_at` `updated_at` `size` `rating` `votes` `revisions` `comments` `random`（约 60s 缓存）。

Data Form 排序：`order="_fieldname"`；数值：`order="_albums::integer desc"`。

## 分页与数量

| 参数 | 含义 |
|------|------|
| `limit` | 总条数上限 |
| `perPage` | 每“分页”条数，默认 20，最大 250 |
| `reverse` | `yes` 时在当前分页内倒序显示 |

## 输出控制

| 参数 | 含义 |
|------|------|
| `separate` | `yes`（默认）：每条单独编译进容器；`no`：拼成单一源码再编译一次——做 `*` 列表或 `\|\|` 表格时用 `no` |
| `wrapper` | 是否外包总容器，默认 yes |
| `prependLine` / `appendLine` | 仅 `separate=no` 时在首尾插入文本（如 `[[tabview]]` / `[[/table]]`） |

### head / body / foot（推荐替代 prepend/append）

```
[[module ListPages category="x" wrapper="no" separate="no"]]
[[head]]
[[ul id="u-list"]]
[[/head]]
[[body]]
[[li]]%%title_linked%%[[/li]]
[[/body]]
[[foot]]
[[/ul]]
[[/foot]]
[[/module]]
```

### separate 语义差异（必记）

| | `separate="yes"` | `separate="no"` |
|--|------------------|-----------------|
| 编译 | 每页一次 | 合并后一次 |
| `iftags` | 看列表项页标签 | 看宿主页标签 |
| `[[image :first]]` | 可用 | 不可靠/无效 |

## 模块体变量（节选）

结构：`%%name%%` `%%category%%` `%%fullname%%` `%%title%%` `%%title_linked%%` `%%link%%`（自定义域慎用）  
父页：`%%parent_*%%`  
内容：`%%content%%` `%%content{n}%%` `%%preview%%` `%%preview(80)%%` `%%summary%%` `%%first_paragraph%%`  
标签：`%%tags%%` `%%tags_linked%%` `%%tags_linked|前缀%%`；隐藏：`%%_tags%%` 等  
表单：`%%form_data{field}%%`（显示值）、`%%form_raw{field}%%`（select/pagepath 内部值；**wiki 字段出链/出语法常用 raw**）、`%%form_label{field}%%` `%%form_hint{field}%%`  
元数据：`%%created_at%%` `%%created_by%%` `%%created_by_linked%%`（及 updated_/commented_ 系列，含 `_unix` `_id`）  
统计：`%%children%%` `%%comments%%` `%%size%%` `%%rating%%` `%%rating_votes%%` `%%rating_percent%%` `%%revisions%%`  
列表上下文：`%%index%%` `%%total%%` `%%limit%%` `%%total_or_limit%%`  
站点：`%%site_title%%` `%%site_name%%` `%%site_domain%%`

日期格式：`%%created_at|%e %B %Y%%`（strftime 风格 token）。

标签链接前缀示例：
- `%%tags_linked%%` → `/system:page-tags/tag/shiny`
- `%%tags_linked|%%` → `/shiny`
- `%%tags_linked|player:%%` → `/player:shiny`

## URL 传参 `@URL`

```
[[module ListPages category="@URL|blog" created_at="@URL" order="@URL|created_at desc"]]
```

访问：`/page/created_at/2008.07/order/rating desc/limit/3`  
`+tag` 在 URL 中写成 `%2b`：`/tags/%2bapple,-banana`

多 ListPages 同页抢 URL 参数时，设不同 `urlAttrPrefix="foo_"`。

## RSS

`rss="标题"` 开启；可选 `rssDescription` `rssHome` `rssLimit` `rssOnly="yes"`。  
注意：RSS **忽略** `created_at` 选择器。

## 与 CountPages

只需数字时用 CountPages（选择器类似，体中 `%%total%%` 或 `%%count%%`），更轻。

## 弃用对照（见到旧码要会译）

| 旧 | 新 |
|----|----|
| `order="dateCreatedDesc"` | `order="created_at desc"` |
| `order="titleAsc"` | `order="title"` |
| `%%linked_title%%` | `%%title_linked%%` |
| `%%author%%` / `%%date%%` | `%%created_by%%` / `%%created_at%%` |
| `categories=` / `tag=` / `date=` | `category=` / `tags=` / `created_at=` |
| `skipCurrent="yes"` | `range="others"` |
| 无 body 的“默认格式” | 始终写显式 body + `[[/module]]` |
