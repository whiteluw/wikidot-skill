# Wikidot 基础语法

## 行内格式

| 语法 | 效果 |
|------|------|
| `//text//` | 斜体 |
| `**text**` | 粗体 |
| `//**text**//` | 斜粗 |
| `__text__` | 下划线 |
| `--text--` | 删除线 |
| `{{text}}` | 等宽 teletype |
| `x^^2^^` | 上标 |
| `H,,2,,O` | 下标 |
| `##blue\|word##` / `##44FF88\|word##` | 预定义色名或十六进制 |
| `[[span style="…" class="…" data-x="…"]]…[[/span]]` | 自定义行内元素 |
| `[!-- comment --]` | 源码注释（不渲染） |

字号：`[[size larger]]…[[/size]]`、`[[size 80%]]`、`[[size 1.2em]]`、`[[size 18px]]`、`xx-small`…`xx-large`。可嵌套。

## 转义

- `@@literal **not** parsed@@` — 双 `@` 原样输出
- `@<…>@` — 通用转义；可放 HTML/Unicode 实体。内部先把 `& < >` 写成 `&amp; &lt; &gt;`
- 在 `[[code]]` 里若要展示 `[[include …]]` 而不执行：在 `[[include` 前加一个空格

## 段落与排版

- 空一行 = 新段落；单换行 = 软换行
- 排版辅助：``` ``quotation'' ``` → “quotation”；`em -- dash`（两侧有空格）→ —
- 水平线：至少四个 `-`：`----`

## 标题与目录

```
+ H1
++ H2
+++ H3
++++ H4
+++++ H5
++++++ H6
+* 不进 TOC 的标题
[[toc]]
[[f>toc]]   (* 右浮动)
[[f<toc]]   (* 左浮动)
```

TOC 会生成书签 `#toc`；各级标题自动生成 `#toc1` 等锚点。

## 列表

```
* 项目
 * 嵌套（星号前加空格）
# 编号
 # 嵌套编号
: 术语 : 定义
```

多行列表项：行尾空格 + `_`，下一行续写（`_` 后不能再有字符）。

### 高级列表（Bootstrap 友好）

```
[[ul class="nav"]]
[[li class="active"]]Item[[/li]]
[[li style="color:red;"]]Item 2[[/li]]
[[/ul]]
```

`[[ul]]`/`[[ol]]`/`[[li]]` 支持 `class`、`style`、`data-*`，可嵌套。

## 引用与折叠

```
> 一级引用 \
续行用反斜杠
>> 二级
```

```
[[collapsible show="+显示" hide="-隐藏" hideLocation="both" folded="no"]]
内容（标签必须独占一行）
[[/collapsible]]
```

- `hideLocation`：`top`（默认）/`bottom`/`both`
- `folded="no"`：默认展开

折叠导航容器常用于 `nav:side`，外观随站点样式而变化。

```
[[div class="foldable-list-container"]]
* 分类
 * [[[page|链接]]]
[[/div]]
```

## 链接

### 内部

```
[[[page-name]]]
[[[category: page name]]]
[[[page|自定义文字]]]
[[[page|]]]              (* 用页面标题作文字)
[[[page#toc1|跳到节]]]
[[[/|首页]]]
```

非法字符会被 unixify 进 URL，显示文字可保留原样。

### 外部 / InterWiki / 邮件

```
[http://example.com 文字]
[*http://example.com 新窗口]
http://example.com          (* 裸 URL 常可自动成链)
[wikipedia:Albert_Einstein]
[wikipedia:zh:页面名 显示]
[google:free+wiki]
support@example.com
[support@example.com 写信]
```

### 锚点

```
[[# section-a]]
[#section-a 回到此处]
```

### 自定义 `<a>`（进阶）

可用 `[[a href="…" class="…" style="…"]]…[[/a]]`；`[[a_ …]]` 会吃掉周围空白，便于拼 Bootstrap。

### Magic URI（URL 控制页行为）

路径参数示例：
- `/noredirect/true` — 跳过 Redirect 模块
- `/norender/true` — 不渲染（抢救坏页）
- `/tags/tag1,tag2` — 经 URL 设标签
- `/title/标题`、`/parentPage/父页` — 配合新建编辑
- Hash：`#_editpage`、`#_edittags`、`#_history`、`#_files`、`#_wantedpages`、`#_orphanedpages`、`#_draftpages`、`#_sitetools`

## 图片与图库

```
[[image source width="200px" height="…" alt="…" title="…" link="page-or-url" style="…" class="…" size="thumbnail"]]
```

对齐前缀：`[[=image` 居中，`[[<image` 左，`[[>image` 右，`[[f<image` / `[[f>image` 浮动。

`source` 可为：URL、本页附件名、`:first`、`/other-page/file.jpg`、`flickr:id`。

`size`：`square` / `thumbnail` / `small` / `medium` / `medium640` / `large` / `original`（后几项偏 Flickr/本地缩放）。

```
[[gallery size="thumbnail" order="name" viewer="yes"]]
: file1.jpg alt="…" link="…"
: /other/file2.png
[[/gallery]]
```

无列表时，图库默认展示**当前页附件**中的图片。

## 表格

### 简单表

```
||~ 表头1 ||~ 表头2 ||
|| 单元格 || 单元格 ||
|||| 跨两列 ||
|| 左 |||| 右跨两列 ||
```

单元格内换行：行尾 `_`。

### 高级表

```
[[table style="border-collapse:collapse;"]]
[[row]]
[[hcell style="…" class="…"]]头[[/hcell]]
[[cell colspan="2" rowspan="1"]]单元格[[/cell]]
[[/row]]
[[/table]]
```

`table`/`row`/`cell`/`hcell` 均支持 `class`/`style`；单元格另支持 `colspan`/`rowspan`。可嵌套。

## 代码 / HTML / 笔记

```
[[code type="php"]]
<?php echo "hi"; ?>
[[/code]]
```

`type` 支持：`php html cpp css diff dtd java javascript perl python ruby xml`。

- `[[code]]` 内 wiki 语法默认不解析，但 **`[[include]]` 仍会执行**（前加空格可阻止）
- 代码块 URL：`/category:page/code`、`/code/2`；含 template：`/code_`、`/code_/2`

```
[[html]]
<div style="…"><p>原始 HTML（iframe 沙箱）</p></div>
[[/html]]
```

```
[[note]]
旁注块
[[/note]]
```

## 数学 / 脚注 / 文献 / 日期

```
[[math label1]]
E = mc^2
[[/math]]
[[$ a^2+b^2=c^2 $]]
```

```
正文[[footnote]]注脚内容[[/footnote]]
[[footnoteblock title="注释"]]
```

```
参见 ((bibcite bell)).
[[bibliography title="参考文献"]]
: bell : Bell et al. //Nature// 1968.
[[/bibliography]]
```

```
[[date 1216153821 format="%e %B %Y|agohover"]]
```

`timestamp` 为 Unix 秒；`|agohover` 显示“多久以前”悬停。

## 文件与用户

```
[[file filename.pdf | 下载 PDF]]
[[file /other-page/doc.zip | 压缩包]]
[[user alice]]
[[*user alice]]   (* 带头像)
```

## 按钮与社交（简）

```
[[button edit text="编辑" class="…" style="…"]]
[[button set-tags +draft -published text="标为草稿"]]
[[social digg,facebook]]
```

`button` 类型含：`edit`、`edit-append`、`edit-sections`、`history`、`print`、`files`、`tags`、`source`、`backlinks`、`talk`、`delete`、`rename`、`site-tools`、`edit-meta`、`watchers`、`parent`、`lock-page`、`set-tags` 等。

`set-tags`：`+tag` 添加，`-tag` 删除，`-*` 清可见标签，`-_*` 清隐藏标签；先删后加。
