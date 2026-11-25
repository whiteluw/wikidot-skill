# Live Template、Include/CSI、Data Forms

## Live Template（`_template`）

为某 category 创建页 `category:_template`（默认 category 则为 `_template`）。  
浏览时：模板源码 + 页面内容合成；编辑时：用户通常只编辑内容区。

最小模板：

```
%%content%%
```

常见增强：

```
[[module CSS]]
/* 本类页面样式 */
[[/module]]

by %%created_by_linked%% on %%created_at|%e %B %Y%%
标签: %%tags_linked%%

%%content%%

----
[[module Rate]]
[[module Comments]]
```

### 内容分段 `%%content{n}%%`

页面正文用 `====`（≥4 个 `=`）分成多段；模板按号取用：

```
(* _template *)
[[div style="float:right;width:200px"]]
%%content{1}%%
[[/div]]
%%content{2}%%
```

对应页面源码：

```
侧栏内容
====
主内容
```

字面量 `====`：写成 `@@====@@`，或在 code 块内用行尾空格转义。

### 默认新建正文

在模板分隔线 `====` 下方可提供**新页面初始内容**（编辑器预填）。分隔线可以写得更长，便于识别。一旦使用，旧的 `template:` 选择机制对该 category 往往不再出现。

### 模板变量

与 ListPages 大量重叠：`%%title%%` `%%content%%` `%%content{n}%%` `%%tags%%` `%%form_data{x}%%` `%%rating%%` `%%created_at%%` …  
日期：`%%created_at|format%%`。

### 隐藏页与 404

- 名以 `_` 开头的页**不套用** live template  
- 全站缺失页文案：`_404`；某类：`category:_404`  
- 变量 `%%404_page_name%%`；可放 `[[button edit]]` 鼓励创建

创建 `_template`：地址栏直接打开 `https://site.wikidot.com/category:_template`（NewPage 模块做不到）。

---

## Include 与 CSI

```
[[include page-name]]
[[include category:page-name]]
[[include :othersite:category:page]]
```

规则：
- include 行前后应有换行；整行基本上就是 include
- 被包含页里的 `{$var}` 由调用方赋值：

```
[[include component:infobox |
title=标题 |
body=正文内容 |
color=red
]]
```

组件页内：

```
[[div class="infobox" style="border-color:{$color}"]]
++ {$title}
{$body}
[[/div]]
```

- **跨站 include = CSI**：复用主题站/组件站的标准做法  
- 图片路径：在组件页写 `[[image component:infobox/icon.png]]`，不要写裸文件名  
- `[[code]]` 内 include 仍会执行；演示代码前加空格

---

## Data Forms

把某一 category 变成“结构化数据库表”。**一个 category 只能有一套 form**；该 category 所有页共享字段定义，编辑器变为表单而非自由 wiki（wiki 字段除外）。

### 建立步骤

1. 创建 `band:_template`（示例 category = `band`）  
2. 上半部分：展示布局（`%%form_data{…}%%` / `%%form_raw{…}%%`）  
3. 一行 `====`  
4. 下半部分：`[[form]]` YAML `[[/form]]`  
5. Site Manager 可对该 category 开 autonumber，减少重名

### YAML 骨架

```
[[form]]
fields:
  field_name:
    label: 显示名
    type: text
    width: 30
[[/form]]
```

硬性：
- 顶层必须有 `fields:`  
- 字段名以字母开头  
- `key: value` 冒号后**必须有空格**  
- YAML 缩进必须整齐（建议 2 空格）  
- 值 `Yes`/`No`/`True`/`False` 必须加引号  
- 可用社区校验工具：http://community.wikidot.com/app:convert  

默认 `type` 为 `text`；若只写 `values:` 则默认为 `select`。

### 字段类型（常用）

| type | 用途 | 备注 |
|------|------|------|
| `text` | 单行文本 | `width` |
| `wiki` | 可含 wiki 语法的多行 | `width`/`height`；列表输出常用 `%%form_raw{field}%%` |
| `select` | 单选；2–4 项常为 radio，≥5 为下拉 | `values:` + `default:` |
| `checkbox` | 0/1 | `default: 1` |
| `url` | URL | 展示用 data；当图源等用 raw |
| `file` | 上传；文件落到另一 category 的页（默认 `file`） | `category:` 可改；**不会**进本页 `[[gallery]]` |
| `pagepath` | 树形选页 | `category` `max-level` `default`；存 fullname |
| `date` | 日期选择 | 见官方 date-field |
| `hidden` | 隐藏值 | |
| `static` | 只读说明 | |
| `password` | 掩码输入 | 内容以明文保存在页面源码中，勿填真实密码 |

select 示例：

```
  type:
    label: Music type
    type: select
    values:
      0: Classical
      6: Rock
    default: 6
  current:
    label: Recording?
    type: select
    values:
      no: "No"
      yes: "Yes"
    default: no
```

### 展示变量

| 变量 | 何时用 |
|------|--------|
| `%%form_data{field}%%` | 普通显示（select 显示标签文字等） |
| `%%form_raw{field}%%` | URL/文件路径、select 内部键、要把字段值再当 wiki 语法解析时 |
| `%%form_label{field}%%` / `%%form_hint{field}%%` | 标签与提示 |

图片示例：

```
[[f<image %%form_raw{bandimage}%% width="150px"]]
++ %%title%%
类型: %%form_data{type}%%
站点: %%form_data{bandwebsite}%%

====

[[form]]
fields:
  type:
    label: Music type
    type: select
    values:
      6: Rock
      0: Classical
    default: 6
  bandimage:
    label: Image
    type: file
  bandwebsite:
    label: Website
    type: url
[[/form]]
```

### 与 ListPages

- 筛选：`_type="6"` 或 `_current="yes"`  
- 排序：`order="_type"` 或 `order="_year::integer desc"`  
- 输出：非 wiki 用 `%%form_data{…}%%`；wiki/路径用 `%%form_raw{…}%%`

### 删除/迁移注意

去掉 form 或改字段名可能导致已有页数据难显示；改 category 结构前先导出/备份页面。删除 form 定义不等于自动清字段存储——按官方 “Deleting a form” 流程操作。

参考 [Wikidot 模板](https://www.wikidot.com/doc:templates)、[Include](https://www.wikidot.com/doc-wiki-syntax:include)及[SCP 中文站数据表单手册](https://scp-cn.wiki/wikidot-data-form-tech)。
