# 常见陷阱与排错

按症状排查。修好后重新预览/保存。

## 解析/结构

| 症状 | 原因 | 处理 |
|------|------|------|
| `[[div]]` 原样显示 | 标签未独占一行，或行尾有空格 | 开闭标签单独成行，去尾部空白 |
| 多出空段落/怪间距 | div/a 周围被当成段落 | 改用 `[[div_ …]]` / `[[a_ …]]` |
| `====` 把模板劈开 | `_template`/分段语义 | 字面量用 `@@====@@` |
| Tab 里锚点/TOC 失效 | TabView 固有限制 | 勿依赖；改用折叠或分頁 |
| 粗体/链接怪异 | 当成 Markdown 写了 | 改用 Wikidot：`[[[page]]]`、`[url 文字]`、`//斜体//` |

## 转义与演示代码

| 症状 | 处理 |
|------|------|
| 想显示 `**` 却被加粗 | 包在 `@@…@@` |
| 想显示 `[[module ListPages]]` | `@<[[module ListPages]]>@` 或 `@@[[module ListPages]]@@` |
| code 块里 include 仍执行 | `[[include` 前加一个空格 |
| 实体不显示 | 用 `@<&copy;>@` 等形式 |

## Include / 图片 / CSI

| 症状 | 处理 |
|------|------|
| include 后图片全挂 | 源页改为 `[[image this-page/file.png]]` |
| 变量未替换 | 检查 `{$name}` 与 `name=value`；分隔用 `\|`；多行参数格式 |
| CSI 不更新 | 缓存/目标站权限；确认 `:sitename:page` 写对 |

## 模块

| 症状 | 处理 |
|------|------|
| ListPages 整段不出现 | 体内是否含 `[[code]]`/`[[html]]`；标签是否成对；category/tags 是否筛空 |
| 表格 ListPages 裂开 | 设 `separate="no"`，并用 `prependLine`/`[[head]]` 包住表头 |
| iftags 判断错对象 | 检查 `separate`；yes=每项页，no=宿主页 |
| `:first` 图空白 | 需要 `separate="yes"` |
| 两个模块分页互相干扰 | 不同 `urlAttrPrefix` |
| Redirect 后无法编辑 | URL 加 `/noredirect/true` |
| 页面白屏/死循环 include | `/norender/true` 进编辑；检查互相 include |
| Rate 不显示 | Site Manager 未对该 category 开评分 |
| NewPage 不能建 `_template` | 地址栏手动创建 |

## Data Forms

| 症状 | 处理 |
|------|------|
| 保存报 YAML 错 | 冒号后加空格；修正缩进；`Yes`/`No` 加引号 |
| 字段不出现 | `fields:` 下字段缩进层级错误 |
| 图片只显示链接 | 布局里用 `[[image %%form_raw{filefield}%%]]`，不要只用 data |
| wiki 字段在列表里乱码/不解析 | ListPages 里改用 `%%form_raw{field}%%` |
| 与普通页混用失败 | 同一 category 不能同时当自由编辑+表单库——拆 category |
| select 筛选无效 | 筛的是 **raw 键**（如 `6`），不是显示名 `Rock` |

## 标签与权限

| 症状 | 处理 |
|------|------|
| TagCloud 看不到某标签 | 是否 `_hidden` 隐藏标签 |
| set-tags 按钮无效 | 当前用户无编辑权；先删后加的顺序 |
| ListPages 看不到页 | `pagetype`/`category`/`tags` 过严；或页在隐藏名 `_` 下需 `pagetype="hidden"`/`*` |

## 评分跨类

不同 category 若混用 `+/-` 与五星模式，按 `rating` 筛选/排序会不准——限制在同一评分模式的 category。

## 快速抢救清单

1. 打开 `/norender/true` 或历史版本  
2. 暂时注释可疑模块：`[!-- [[module …]] --]`（注意注释语法）或删掉  
3. 验证块级标签成对、独占行  
4. ListPages：去掉体内 code/html；改 `separate`  
5. Data Form：丢进 http://community.wikidot.com/app:convert 校验  
6. 仍失败：最小化复现（只留标题+一段+一个模块）再逐步加回
