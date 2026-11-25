# 后室中文主站布局：BHL / 阈限

适用站点为后室中文主站（`backrooms-wiki-cn.wikidot.com`）的 Liminal 基础主题。

页面区域、CSS 层叠和常见组件的 DOM 见[通用页面与组件结构](page-layout.md)。

具体页面的主题 include、页面 CSS 和组件可以覆盖基础规则。以该页面实际加载的依赖为准。

## Black Highlighter 与阈限

**Black Highlighter（BHL）是另一套 Wikidot 主题体系；阈限（Liminal）是基于 BHL 的后室主题。** 阈限由 Lynch 与 Etoile 制作，并有后续维护修改。

后室中文站的阈限基础资源包括：

- `https://github.backroomswiki.cn/Old_BHL/css/normalize.css`
- `https://github.backroomswiki.cn/Old_BHL/css/black-highlighter.css`
- `https://github.backroomswiki.cn/Old_BHL/css/liminal.css`
- `https://github.backroomswiki.cn/Old_BHL/css/sidebar.css`

阈限定义 `--theme-base: "black-highlighter"`、`--theme-id: "liminal"` 和 `--theme-name: "Liminal Theme"`，使用米黄底色、深灰褐站头和含 Inter、Noto Sans SC、Noto Sans TC 的字体栈。

| 变量 | 含义 |
|---|---|
| `--body-font` | 正文字体栈 |
| `--header-font` / `--title-font` | 站头与标题字体栈 |
| `--mono-font` | 等宽文字字体栈 |
| `--logo-image` | 标志图片 |
| `--header-title` / `--header-subtitle` | 主题使用的站名与副标题字符串 |
| `--white-monochrome`、`--gray-monochrome`、`--black-monochrome` 等 | 基础配色分量 |
| `--bright-accent`、`--medium-accent`、`--dark-accent` 等 | 强调色分量 |

配色变量中的 `237, 233, 223` 等值是供 `rgb()`/`rgba()` 组合使用的通道分量，不是独立完整的 CSS 颜色表达式。主题变量需要被其他声明引用才影响外观；仅定义一个变量不会自动改变元素。

后室中文站的折叠侧栏样式在桌面使用 `position:sticky` 和横向 transform 将侧栏移出左边缘，`:hover`、`:active`、`:focus-within` 对应展开状态。其外层、内容区和主内容区分别使用 Grid/Flex 等布局，与 Sigma-9 的绝对定位侧栏不同。窄屏另使用移动导航和 fixed 侧栏。

阈限也引用 Sigma-9 仓库的 `modules/colstyle.min.css`。复用这个折叠模块不改变阈限基于 BHL 的关系。主题身份由整体样式依赖决定，不由某个资源地址中的关键词决定。


## 本站正文样式与组件

- `lightblock`、`darkblock`、`styled-quote`、`dark-styled-quote` 属于本篇阈限主题语境中的样式类，不是 Wikidot 内置关键字，也不保证其他主题或站点可用。
- `.scp-image-block` / `.scp-image-caption` 可在本站使用；类名含 scp 不表示该页属于 SCP 或使用 Sigma-9。BHL 衍生主题的图片框与图注可使用 Flex 结构和内阴影，最终仍以该页规则为准。
- 评分、脚注、折叠、TabView 的结构可以与 SCP 页相同，但颜色、间距、宽度、字体与响应式表现不能因此照搬。著作信息面板也取决于文章实际加载的组件。

## 与 SCP 布局的边界

- 不把 SCP 中文 Sigma-9 的 980px / 1040px 宽度或绝对定位侧栏套到本站。
- 本站桌面折叠侧栏的 sticky / transform / hover 行为，不代表 SCP 中文站默认侧栏也如此。
- 页面若另行采用其他主题，按实际主题解释；“后室中文文章”不等于“必定使用阈限”。
- 比较两种主题时，另见[SCP 中文 Sigma-9](layout-scp-cn.md)。

[后室中文站阈限主题说明](https://backrooms-wiki-cn.wikidot.com/theme:liminal)。
