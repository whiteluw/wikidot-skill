# SCP 中文站布局：Sigma-9

适用站点为 SCP 中文站（`scp-wiki-cn.wikidot.com`）的 Sigma-9 基础主题。

页面区域、CSS 层叠和常见组件的 DOM 见[通用页面与组件结构](page-layout.md)。

具体页面的主题 include、页面 CSS 和组件可以覆盖基础规则。以该页面实际加载的依赖为准。

## Sigma-9

**Sigma-9（SCP-Sigma-9）是为 SCP Foundation 制作的 Wikidot CSS 主题。** 其主题署名为 2014 Wikidot Theme，由 Aelanna 创建，Dr Devan 修改；中文版本另署 MScarlet、Dr Devan 与 Sekai_s。它不是 Wikidot 语法版本、编辑器或独立 CMS。

SCP 中文站的 Sigma-9 样式入口为 `https://sigma9.scpwikicn.com/cn/cn/sigma9_ch.min.css`，维护仓库为 https://github.com/scp-cn-tech/sigma9 。`component:theme` 页面内展示的旧代码与维护版本并不等同。

经典 Sigma-9 外观包括红黑色站头、左侧标志与站名、红色链接、浅色正文背景和桌面双栏。中文站基础规则中：

- `#header` 与 `#top-bar` 最大宽度为 980px；站头内容高 140px，底部另留 22px；顶栏位于站头下沿。
- `#content-wrap` 最大宽度为 1040px，并作为侧栏定位的参照。
- 桌面 `#side-bar` 使用绝对定位，`#main-content` 通过左边距让出侧栏空间。
- 正文字体栈包含 Verdana、Arial、Helvetica；标题、链接、引用、图片框和 TabView 另有各自的样式。
- 窄屏隐藏桌面导航，显示移动导航；侧栏改为移出左侧的 fixed 区域，菜单入口与 `:target` 状态关联。

这些是该主题的基础规则，页面 CSS、组件和不同语言分部的版本可以覆盖它们。“Sigma-9 衍生版式”指在此主题基础上调整样式的版式，不是所有红黑配色页面的统称。


## 本站正文样式与组件

- Sigma-9 的 `.scp-image-block` / `.scp-image-caption` 可使用边框和阴影；外框浮动、宽度与图注样式受该页 CSS 共同影响。不要套用后室阈限的 Flex / 内阴影说明来断定本站实际样式。
- `.scp-image-block` 也被其他站点复用；类名本身不能证明页面使用 Sigma-9。
- 评分、脚注、标准折叠和 TabView 可沿用通用结构，但主题决定视觉样式。著作信息面板不是每篇 SCP 文章都必有；按实际加载的组件识别。

## 与后室布局的边界

- 不把后室中文阈限的 `Old_BHL` 资源、配色变量、字体栈或 sticky / hover 折叠侧栏当作本站默认配置。
- `lightblock`、`darkblock`、`styled-quote` 等类不能因为在后室文档中有定义，就当作 Sigma-9 的内置能力。
- BHL 是独立主题体系，不等于“后室专用语法”；SCP 页面若明确使用 BHL 或其他页面主题，应以该页依赖为准，不能强行套用本篇 Sigma-9 布局，也不能反过来套用后室中文阈限配置。
- 比较两种主题时，另见[后室中文阈限](layout-backrooms-cn.md)。

[SCP 中文站主题说明](https://scp-wiki-cn.wikidot.com/component:theme) · [Sigma-9 维护仓库](https://github.com/scp-cn-tech/sigma9)。
