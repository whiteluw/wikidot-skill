\---  
name: wikidot-syntax  
description: 编写、解释和排查 Wikidot 页面源码与样式。适用于 Wikidot 标记、链接、图片、表格、折叠、TabView、CSS、ListPages、Include、Live Template、Data Forms，以及后室中文阈限与 SCP 中文 Sigma-9 主题。  
\---

# Wikidot 语法与页面开发

1. 根据用户提供的源码、页面地址或站点名称确定目标。平台语法通用。组件、选择器、字体及侧栏行为以目标页实际加载的主题和 CSS 为准。只问通用语法时使用通用示例。
2. 按任务阅读下方参考资料。遇到未知参数、版本差异或站点自定义组件时，对照 Wikidot 文档或目标页源码。把已确认的行为和需要在目标站预览的部分说明清楚。
3. 给出可粘贴的 Wikidot 源码。保留用户原有正文、主题与组件结构，按需求提供修改片段或完整页面。示例依赖附件、分类、组件页或站点配置时，说明需要替换或准备什么。
4. 排查时判断故障发生在源码解析、模块选择、主题 CSS 还是 iframe 文档。优先找出最小修改，说明预览时应观察到的结果。发布与保存由用户的实际请求决定。
5. 面向用户回答时，将本技能及其参考资料作为内部工作指南，直接围绕用户的问题，用自己的话自然解释、给出结论或源码。不要复述技能说明，不要使用“根据本技能”“SKILL.md 中提到”“参见技能的某个章节”等元叙述，也不要无故提及技能名称、章节、文件路径或内部查阅过程。只有用户明确询问技能本身、其内容或实现时，才介绍这些信息。除非用户要求，无需提供依据及查阅Wikidot文档。在需要提供依据时，优先引用 Wikidot 官方文档、目标站点页面或实际源码，并如实说明不确定性。

## 按需参考

- [基础语法](references/basics.md)涵盖标题、列表、链接、附件、表格、脚注和转义。
- [进阶布局](references/layout-advanced.md)涵盖 div、span、对齐、TabView、CSS 和嵌入。[页面与组件结构](references/page-layout.md)说明这些元素渲染后的 DOM 和样式作用域。
- [ListPages](references/listpages.md)说明筛选、排序、分页、输出变量和编译模式。[常用模块](references/modules.md)涵盖评分、评论、新建页面、跳转和计数。
- [模板、Include 和数据表单](references/templates-forms.md)说明跨站引用、分类模板和结构化字段。
- [实用示例](references/examples.md)提供文章骨架、组件、列表和表单片段。[常见陷阱](references/pitfalls.md)按症状组织排查方法。
- [后室中文阈限](references/layout-backrooms-cn.md)适用于后室中文主站的 Liminal 主题。[SCP 中文 Sigma-9](references/layout-scp-cn.md)适用于 SCP 中文站的 Sigma-9 主题。仅在目标页使用对应主题或用户要求比较时查阅。

## 编写源码时留意

- 标题用 `+`、`++`，内链用 `[[[page-name|显示文字]]]`，外链用 `[https://example.com 显示文字]`。块级开闭标签通常各占一行，保持配对，避免行尾空格。
- 连续列表和表格需要 ListPages 将各条结果合并编译时，用 `separate="no"`。需要按各列表项判断标签或读取首张附件图时，检查 `separate="yes"` 的语义。
- Include 片段引用自身附件时，写出附件所属页名。跨站组件的可用性取决于站点访问与实际依赖。
- 分类模板和数据表单的 `====` 有分隔语义。改变分类模板或表单字段前，注意同分类现有页面的渲染与编辑行为。
- 页面 CSS 会作用于宿主页。优先用组件根类限制样式范围，依据页面实际输出处理 `u-` 前缀和主题选择器。独立 HTML 使用 `[[html]]` 进入 iframe，内部不解析 Wikidot 标记。

## Wikidot 文档

- [Wiki 语法](https://www.wikidot.com/doc-wiki-syntax:start)
- [模块目录](https://www.wikidot.com/doc-modules:start)
- [Live Template](https://www.wikidot.com/doc:templates)
- [Data Forms](https://www.wikidot.com/doc-data-forms:start)
