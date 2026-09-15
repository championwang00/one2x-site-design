---
name: one2x-site-design
description: One2X 企业视频服务官网（enterprise.one2x.ai，源码 one2x-enterprise）的页面设计规范。凡是要给这个官网新增页面、新增板块、改版块、做落地页/活动页/案例详情页/方案页，或用户说「按官网风格」「和官网保持一致」「One2X 官网」「企业官网」时都要先读本 skill：它规定了统一的 header/footer、设计 token、板块语法和组件库，保证新页面和现有首页是同一套东西。
---

# One2X 官网设计规范

这份规范从 `one2x-enterprise` 项目现有代码里提炼。目标只有一个：**任何人（包括未来的你）给这个官网加页面，用户一眼看不出是后加的。** 头尾必须原样复用，中间板块必须从下面的语法和组件里拼。

## 0. 先读源码，再动手

规范不能替代代码。开工前先打开这几处，它们是本规范的「真源」：

| 文件 | 看什么 |
|---|---|
| `src/App.jsx` | `<header>` 与 `<footer className="site-footer">` 的完整 JSX；`nav` 数组；`Brand` / `Button` / `SectionTitle` 三个共享小组件；各 `<section>` 的顺序与 class 组合 |
| `src/styles.css` | 按锚点搜，不要按行号（行号每次改动都会失准）：`:root{--ink:` 是营销页真正生效的 token；`header{position:sticky` 是 header 基础，`header.is-scrolled{` 是滚动态；`.site-footer{` 起是整段 footer；注释 `/* Section labels use the selected Voxt pill treatment. */` 附近是胶囊，`/* White cards on soft gray section bands` 附近是灰带上的卡片，`/* Gray bands share a fine diagonal hatch */` 是斜纹；文件末尾带日期注释的段落是历次增量 |
| `src/tokens.css` | Figma 同步的 One2X/Medeo 品牌 token（圆角 `--shape-radius-*`、间距 `--space-s*`）。**颜色部分营销页不用**——页面色板以 styles.css 的 `:root` 为准 |
| `src/components/ui/` | 现成组件：`keyline-icon.jsx`（图标映射）、`value-proof.jsx`（对比表 / 数据条 / MCN 卡）、`stats-bento.tsx`、`process-flow.tsx`、`logo-marquee.tsx`、`case-marquee.jsx`、`digit-pop.jsx`、`dot-pattern.tsx` |
| `public/images/one2x-wordmark.svg`、`public/fonts/` | 品牌字标与自托管字体，新页面直接引用同路径 |

项目是 **React 19 + Vite + 原生 CSS**。Tailwind 只挂了 theme/utilities 层做兜底，页面布局全部走 `styles.css` 里的 class。不要为了新页面引入 shadcn、Radix、CVA 或第二套 token；不要用 Tailwind utility 拼版式。

## 1. 视觉基调（一段话记住）

白底、编辑感、近乎单色。参考对象是 Voxt 官网与 www.one2x.ai：Manrope 粗体负字距标题，大留白，细 1px 线，灰白两种底色交替，灰带上叠一层 45° 细斜纹。**几乎没有彩色**——整页只有黑、三级灰和白；`--accent` 紫色只出现在焦点环和状态点上，新元素一律不引入新色相（合作方 / 平台 logo 用彩色官方版是唯一例外）。无投影、无渐变、无玻璃拟态。

## 2. Token

直接用 CSS 变量，不手写 hex；下面的值是为了让你知道它长什么样。

**颜色**（`styles.css :root`）

```
--ink   #191919   正文与标题、主按钮底
--muted #626262   副文案、说明、eyebrow（另有 #555 / #666 / #60616a 用于导航、胶囊等更轻的层级）
--line  #ebebeb   所有 1px 分割线与描边（卡片描边也可用 rgb(0 0 0 / 7%)）
--paper #fff      白底
--soft  #fafafa   灰带底
--accent #7462bd  只用于 :focus-visible 外框与 .status-dot
```

**字体**：`--font: Manrope, 'PingFang SC', 'Microsoft YaHei', sans-serif`，全站正文与标题。`Nohemi` 只用于品牌字标和 footer/超大数字。`Source Serif 4 Italic` 只用于 footer 大标题里的 `<em>`。字体已自托管在 `public/fonts/`，`index.html` 预加载 Manrope 400/600。

**字阶**（桌面 / ≤960 / ≤700）

```
--display 64 / 56 / 43px   h1（首屏）
--heading 40 / 34 / 29px   h2，letter-spacing -1.5px，line-height 1.4
--title   20px             h3，line-height 1.5
--body    15px             p，line-height 1.9
--small   13px             按钮、导航、说明
```

标题 600 字重，正文 400。h2 允许 `<br/>` 手动断两行。**标题末尾不加句号、逗号**（用户明确要求）。

> 已知不符：首页 `#contact`（「从清晰需求开始。」）与 `#process`（「再进入规模化交付。」）两处标题带句号，早于本规则。新页面一律不带标点；复用 contact 段时把句号去掉——这会让新页面的 contact 标题和首页差一个字符，是当前唯一允许的头尾偏差。要彻底消掉，得改首页文案，等用户拍板。

**圆角**：按钮 / 胶囊 / 标签 = `--shape-radius-full`；卡片 = 16–24px（`--shape-radius-16` / `-20` / `--radius` 24px）；大容器 26px；头像 50%。

**间距**：板块上下 `--section-space` 110 / 80 / 64px；内容宽 `.shell{width:min(1216px,calc(100% - 64px))}`；卡片内边距 20–32px；网格 gap 16–18px。断点两个：**960px、700px**。

**灰带斜纹**：`.tinted` 或指定 id 的 section 用 `background:#fafafa` + `:before` 上 `repeating-linear-gradient(-45deg,transparent 0 7px,rgb(25 25 25 / 4.5%) 7px 8px)`，内容 `position:relative;z-index:1`。

## 3. Header（原样复用，不允许改样式）

结构（来自 `App.jsx`）：

```jsx
<header className={scrolled||menu?'is-scrolled':undefined}>
  <div className="header-inner">
    <Brand/>                                    {/* wordmark.svg 105×28 + <small>企业视频服务</small> */}
    <nav aria-label="主导航">{nav.map(([id,label])=><a href={`#${id}`}>{label}</a>)}</nav>
    <div className="header-actions">
      <Button onClick={openBrief}>提交需求</Button>   {/* 黑色胶囊 + 12px 外向箭头 */}
      <button className="icon-button menu" aria-label="打开导航" aria-expanded={menu}>…</button>
    </div>
  </div>
  {menu&&<nav className="mobile-nav">…</nav>}
</header>
```

行为规则：
- `position:sticky; top:0; z-index:40`；高 76px（≤700 为 68px）。
- 顶部**透明无边框**；`window.scrollY>8` 加 `is-scrolled` → `rgba(255,255,255,.94)` + `backdrop-filter:blur(16px)` + 底线 `rgba(235,235,235,.65)`。播放视频弹窗打开时 header 去掉 blur（见 styles.css 第 12 行注释，避免 Chrome 闪烁）。
- 导航 5 项固定：精选案例 / 解决方案 / 服务流程 / 企业优势 / 常见问题，13px `#555`，gap 28（≤960 为 16）。**新页面的导航链接指回首页锚点**（`/#cases` 这种），不要为子页面另造一套导航。
- ≤700：桌面 nav 隐藏，出现 44px 圆形 `.icon-button.menu`；`.mobile-nav` 纵向列表，边框顶线。
- 主按钮在 header 里缩为 `padding:10px 16px;min-height:40px`，≤700 隐藏箭头。

做第二个页面时，把 `Brand`、`nav`、header JSX 抽成 `src/components/site-header.jsx`，两个页面共用，而不是复制粘贴。

## 4. Footer（原样复用，不允许改样式）

```jsx
<footer className="site-footer">
  <div className="site-footer-inner reveal" data-reveal>
    <div className="site-footer-top">
      <div className="site-footer-brand">
        <a href="#home" className="site-footer-logo"><img src="/images/one2x-wordmark.svg" alt="One2X"/></a>
        <h2>From intelligence to <em>infinity</em></h2>
      </div>
      <nav aria-label="产品链接" className="site-footer-links">
        <a href="https://www.medeo.app/" target="_blank" rel="noopener"><span>Medeo</span><OutwardArrow/></a>
        <a href="#home"><span>企业视频服务</span><OutwardArrow/></a>
      </nav>
    </div>
    <p className="site-footer-copy">© 2025 All rights reserved</p>
  </div>
</footer>
```

规则：纯黑 `#000` 整块，`min-height:720px`（≤960 560，≤700 auto），`padding:80px 0`（64 / 48）。字标 `filter:invert(1)` 反白 97×26。大标题 64px（48 / 36）Manrope 600，`<em>` 切 Source Serif 4 斜体。右侧链接列宽 208px 右对齐，18px 500 字重，hover 时 `opacity:.5` 且箭头 `rotate(45deg)`。版权行 14px `opacity:.5`。footer 前面通常接一段白底 contact section（「开始合作，从清晰需求开始」+ 二维码卡），新页面末尾沿用这一对。

## 5. 板块语法

一个页面 = header + 若干 section + contact + footer。每个 section 遵守：

1. **底色交替**：白 `#fff` ↔ 灰 `#fafafa`（灰带加斜纹）。相邻两段共用一条分割线，不要叠两条（参考 `#core-services + #cases{border-top:0}`）。
2. **标题三件套**：`eyebrow.section-kicker` 胶囊（英文小标签，如 `Why One2X`、`Selected work`）→ `h2` → 可选 `p.muted` 一句描述，整体居中、`max-width:740px`。左对齐变体只在解决方案选择器这种左右分栏里用。
3. **内容宽度**：包在 `.shell` 里；需要通栏底色时 section 自己 `width:100%` + `padding-inline:max(32px,calc((100% - 1216px)/2))`。
4. **进场**：内容容器加 `className="reveal" data-reveal`，滚动到 8% 可见时加 `is-in`，400ms `--ease-out-quart` 上浮 16px；列表类容器再加 80ms 延迟。`prefers-reduced-motion` 下全部取消。
5. **顺序参考**（首页）：hero → 核心业务 bento → 核心服务 → 精选案例 → 谁需要我们 → Why One2X（对比表 / 数据条 / MCN 卡）→ 合作企业 → 解决方案 → 服务流程 → 团队优势 → FAQ → contact → footer。新页面挑其中的模式复用，不发明新的板块形态。

## 6. 组件库

下面每一条都是代码里已有的 class / 组件，直接用；样式不够时在 `styles.css` **末尾**追加一段带日期注释的规则，不改上面的。

**按钮 `.button`**：黑底白字胶囊，`padding:12px 20px; min-height:46px; font-size:13px`，右侧 12px `OutwardArrow`（hover 转 45°），`:active` 缩 .97。`.button.secondary` 白底黑字灰描边。带播放图标用 `icon="PlayArrowIcon"`。一个板块最多一主一次两个按钮，放在 `.actions`（gap 12，居中）。

**圆形图标钮 `.icon-button`**：44×44，灰描边白底，用于关闭 / 上一下一 / 菜单。

**胶囊标签**：`.eyebrow.section-kicker`（板块小标签，12px `#60616a`，白 80% 底 + 8% 黑描边）；`.pill`（首屏用，灰底）；`.mcn-tag`（内容标签，透明底 + `--line` 描边 + `#4a4a4a`，11px）；`.audience-tags span`（受众胶囊，带小圆图）。**标签不用彩底。**

**卡片**：白底、`1px solid rgb(0 0 0 / 7%)` 或 `--line`、圆角 16–26px、无阴影。hover 允许 `translateY(-3px)` + 描边加深到 `#dcdcdc`，可带一层极淡阴影 `0 8px 28px rgba(25,25,25,.06)`。灰带上的卡用白底，白底上的卡用白底加描边或 `#f5f5f5` 底。

**Bento / 数据卡**（`.stats-bento`、`.business-bento`、`.benefits`）：网格 gap 14–18，大数字 44–64px 500 字重负字距，`DigitPop` 做数字弹入；小标题 15–18px，说明 12–13px `--muted`。

**对比表 `.compare-table`**：三列（项 / 传统 / One2X），`table-layout:fixed`，One2X 列 `#f6f6f7` 底 + 500 字重，表头带黑色小圆点 `.compare-badge`；外框 `--radius` 圆角；≤700 缩到 11.5px 并允许 `overflow-x:auto`；表下一行 12px 灰色小字说明。

**头像卡 `.mcn-card`**：48px 圆头像 + 名称 / handle 竖排 + 右上角 18px 平台彩色 logo + 标签 + 2–3 行 13px 说明；整卡 `<a target=_blank>`。栅格 4 / 2 / 1 列。

**FAQ**：左标题 sticky，右侧一个连续白色圆角容器，每项 `border-bottom:1px solid --line`，按钮 16px 600，chevron 旋转 180°，答案用 `grid-template-rows:0fr→1fr` 展开。

**方案选择器 / 流程 / logo 墙 / 案例流**：直接复用 `solution-tabs`、`ProcessFlow`、`LogoMarquee`、`CaseGallery`，不要重做。

**弹窗 `Modal`**：原生 `<dialog>`，圆角 24，`modal-in` 240ms 缩放进场，关闭后焦点还给触发元素；表单用 `.brief-form`（字段 1px `#ddd` 描边 8px 圆角）。

**卡片封面用图**：只在有题材匹配的素材时才配图。`images/cases-16`、`images/blog-covers` 是首页案例与受众用的成片截帧，题材对不上时硬套会同时坏两件事——语义不对，而且这些截帧自带色彩，直接违反 §1 的近乎单色。没有匹配素材就走**纯文字卡**：`01`–`06` 编号（12px `#aaa`）+ h3 + 说明 + `.mcn-tag`，对齐 `.quality-grid` 的编辑感（参考 `.seo-card`）。宁可没有图，也不要错的图。

**图标**：`<Icon name="…"/>` 走 `keyline-icon.jsx` 的 `@keyline-icons/react` 映射；外向箭头统一 `<OutwardArrow/>`。不要另引 lucide 或手绘 SVG。品牌 logo 一律用官方资产或来源可查的图标包（`@iconify-json/logos`、`simple-icons`），不凭记忆描。

## 7. 动效尺度

只有三种：页面进场 `page-enter`（320–380ms）、滚动 `reveal`（400ms）、微交互（按钮 150–200ms、卡片 hover 350ms）。缓动统一 `--ease-out-quart: cubic-bezier(.165,.84,.44,1)`。首屏图片流 18s 线性循环、离屏暂停、`prefers-reduced-motion` 暂停。别加视差、别加自动轮播文字。

## 8. 新页面流程

1. 读 §0 列的文件，确认要复用哪些 section 模式。
2. 抽出 `site-header.jsx` / `site-footer.jsx`（若还没抽），新页面导入，不复制。**`Modal` 和 `BriefForm` 要一起抽**——header 的「提交需求」按钮依赖它们，只抽头尾的话子页那颗按钮点不开。现成位置：`src/components/site/`（`primitives.jsx` 放 Brand / Button / SectionTitle / useReveal / usePlayIntro / Modal，`brief-form.jsx` 放需求弹窗）。`SiteHeader` 收一个 `home` prop，子页传 `'/'`，导航就变成 `/#cases` 指回首页锚点。
3. 按 §5 排板块：先写 JSX 结构，class 全部取自现有；数据放 `src/*.json`（照 `works.json`、`mcn-cases.json` 的做法）。
4. 只在 `styles.css` 末尾追加规则，注释写日期和用途；新 class 用板块前缀（`.mcn-`、`.compare-` 这种）。
5. 文案：标题不带句读；数字用「300+」「860W+」「87%」这类短形式；说明 ≤ 2 行。
6. 自查（§9），`npm run build` 通过，截 1440 / 390 / 320 三档给用户看，等确认再谈部署。

## 8.5 SEO 页 / 独立落地页

关键词落地页、活动页、方案页走独立 HTML 入口，不是 SPA 路由——正文之外还要交付一份能被抓取的 head。

**构建**：多页 Vite。页面目录放在仓库根，路径即 URL（`solutions/ai-ecommerce-video/index.html` → `/solutions/ai-ecommerce-video/`）；
`vite.config.mjs` 的 `build.rollupOptions.input` 里加一条入口；React 入口放 `src/pages/<name>-entry.jsx`，页面组件放 `src/pages/<name>.jsx`。
入口要 import 首页那一整套样式：`tailwind.css` / `styles.css` / `tokens.css` / `motion.css` / `transitions.css` / `success-check.css`，少一个就掉 token 或掉动效。

**head 必备**（写在那个 `index.html` 里，不要靠运行时塞）：

- `<title>` 主关键词 + 修饰词 + 品牌，控制在 30 个汉字内
- `description`（80–120 字，带数字和交付承诺）、`keywords`
- `canonical` 指向带结尾斜杠的正式 URL
- `og:type/site_name/locale/title/description/url/image` + `twitter:card`
- JSON-LD `@graph`：`BreadcrumbList` + `Service`（含 `hasOfferCatalog` 列可交付形态）+ `FAQPage`
- **JSON-LD 的 FAQ 文案必须和页面上 FAQ 一字不差**，两边不一致会被判结构化数据造假

**页面内**：H1 全站唯一且含主关键词；面包屑用 `.seo-breadcrumb`（首页 / 板块 / 当前页），分隔符用 `<i aria-hidden>`，当前页 `<span aria-current="page">`；
结尾放一段内链板块（`.seo-links`）指回首页锚点，让权重回流；板块顺序照 §5 的白灰交替拼，不发明新形态。

**已知缺口**：项目是 React CSR，正文不在静态 HTML 里。meta 和 JSON-LD 静态输出没问题，但正文要可抓需要加预渲染（vite-plugin-prerender 或 SSR），
目前还没有。另外仓库没有 `sitemap.xml` / `robots.txt`，每加一个 SEO 页都应同步补一条。这两件都是**已知欠账，不是做完了**。

## 9. 一致性自查

- 头尾是不是和首页逐像素一致（透明→白 blur 的 header、黑 footer、同一组导航）。
- 页面上除黑白灰、平台 logo 外有没有第二种颜色。
- 有没有阴影、渐变、非胶囊的按钮、非 16–26px 圆角的卡。
- 新增的字号是不是落在 64 / 40 / 20 / 18 / 15 / 13 / 12 / 11 这条阶梯上。（**存量不符**：现有 `styles.css` 里有 38 种不同字号，这条是对新代码的要求，不是对现状的描述。）
- 灰带有没有斜纹，相邻 section 有没有叠边线。
- 每个内容块有没有 `reveal`，reduced-motion 下是否直接显示。
- 320px 无横向溢出，所有图片有 `alt`（装饰图 `alt=""`）。
- 标题末尾无标点（含复用来的 contact / process 标题）。
- 卡片配图题材是否真的对得上；对不上就换成纯文字卡。
- SEO 页：title / description / canonical / OG / JSON-LD 齐了吗，JSON-LD 的 FAQ 和页面上是否逐字一致。

## 10. 本 skill 的维护（项目整体规则）

**这个官网只有这一个设计 skill。** 新需求往里改，不另建 skill、不写并行文档——并行规范一出现，执行的人就要判断听谁的，而这种判断迟早会判错。

**唯一真源是仓库内的这个文件**（`one2x-ai/one2x-enterprise` 的 `.claude/skills/one2x-site-design/SKILL.md`）。Claude 账号级 skill、ChatGPT / Codex 侧（根目录 `AGENTS.md` 指向这里）、以及公开镜像仓库 `championwang00/one2x-site-design`，都是它的镜像。改动顺序固定：**先改仓库这份 → commit → 再同步账号级 skill → 再同步公开镜像仓库**。各处不一致时一律以本仓库为准。

**何时更新**：新增可复用组件或板块形态、用户否决了某做法、定下新约束、发现某条已和代码对不上——落地后就更新，不攒批。

**更新时要做四件事，不是只记录**：

- **更新**：写进对应章节，同时检查有没有因此失效的旧条目，一并改掉。
- **反思**：这次为什么要改？是 skill 没覆盖到（缺漏）、写错了（误导）、还是执行时没读（触发问题）？三种病因对应三种药。
- **反馈**：主动说出代价——这条新规则让什么变难、和哪条冲突、哪些现存页面不符合。不报忧的规范会越写越厚且越来越假。
- **评判**：分清「原则」和「一次性偏好」。判断依据：换一个页面、换一个板块，这条还成立吗？不成立的只写进当次交付说明，不要污染规范。

**复盘 Agent 需要的输入**：用户原话纠正（verbatim，不要二手转述）、本轮代码 diff、skill 当前全文、已知失效点。产出是完整的 SKILL.md 替换稿 + 一段「改了什么、为什么、砍掉了什么」。

**推送**：Cowork 沙盒是隔离 Linux VM，拿不到 macOS 钥匙串，不能 push。写好并 commit 后由本人或 macOS 本地已授权的 agent 推送。

## 11. 变更记录

### 2026-09-15 · 第一次拿本规范做新页面（`/solutions/ai-ecommerce-video/`）

**改了什么**：§2 补标题标点的已知不符点；§6 新增「卡片封面用图」规则；§8.2 补 Modal / BriefForm 一起抽；新增 §8.5 SEO 页专章；§9 自查清单加三条。

**反思**——三种病因各占一部分：

- *缺漏*：SEO 页是全新场景，规范此前只讲板块和组件，没讲 head、JSON-LD、面包屑、多页入口。执行时全靠临场补，下一个人不一定会补。→ 开专章。
- *误导*：§2 的「标题末尾不加标点」和首页实际代码冲突，而 §3/§4 又要求头尾原样复用。两条规则一起执行会打架，执行者只能自己挑一边。→ 把冲突写明，并给出「新页面不带标点」的裁决。
- *触发问题*：§6 说「直接用现成组件 / 素材」，读的时候不会意识到「现成素材题材不匹配」也算不能用。结果是配了六张与电商无关的成片截帧，其中一张粉色画面，直接违反 §1。→ 把「宁可没有图」写成明规则。

**反馈（代价）**：

- §8.5 让 SEO 页的开工成本明显变高——head + JSON-LD 大约是页面本身工作量的三成，而且 JSON-LD 和页面 FAQ 必须双向同步，以后改一句 FAQ 就要记得改两处。这个重复是真实负担，但结构化数据不一致的风险更大，暂时接受。
- 「宁可没有图」会让新板块普遍更素。首页靠案例封面撑视觉，子页照这条走会显得更冷。可接受，但如果之后有电商题材素材，形态卡应该补回配图。
- §8.2 把 Modal / BriefForm 抽出来动了 `App.jsx`，首页的 diff 不算小。收益是第二个页面起步接近零成本，但**下一次给首页做改动的人要知道头尾已经不在 App.jsx 里了**。

**评判（原则 vs 一次性偏好）**：

- 原则（换页面、换板块都成立）：§8.5 整章、§6 的封面用图规则、§8.2 的 Modal/BriefForm、§9 新增的三条自查。
- 一次性偏好（不进规范）：本次选的电商题材、六种形态的具体文案、`01`–`06` 的编号写法只是 `.quality-grid` 编辑感的一种实现，不强制。
- 悬而未决：首页 contact / process 两处句号是改文案还是改规则，需要用户拍板。在拍板前，规范给的是「新页面不带标点」这个可执行的临时裁决，不是最终答案。

### 2026-09-15 · 加 CI 强制层

**改了什么**：新增 §12（CI 检查）；§9 把「字号全在阶梯上」改成如实说法。

**反思** —— 病因是*误导*：§9 原文把「字号全在 64/40/20/18/15/13/12/11 上」当事实写，但实测 `styles.css` 现存 **38 种**字号、**94 种** hex、**11 处**投影。规范在描述一个理想化的站，不是这个站。照原文自查的人会发现整份代码都"不合规"，然后合理地怀疑整份规范。→ 把存量不符写明。

**反馈（代价）**：

- CI 会开始拦人，包括拦你自己。想在活动页上用一抹彩色时会红灯——留了 `design-check-ignore` 逃生口，但那要求你写明理由，这本身是摩擦。
- 只查新增行意味着**存量永远不会被强制收敛**。94 种 hex 不会因为有了 CI 就变少，只是不再增长。真要收敛得单独排一次。
- 第一版检查差点上线就出事：它把 §6 明文批准的卡片 hover 阴影和 inset 描边都判成违规，4 处误报全是合规代码。**检查必须精确编码规范，而不是规范的简化版**——否则被怀疑的是规范。

**评判（原则 vs 一次性偏好）**：

- 原则：CI 只查新增行、只查能机器判定且不会误伤的规则、必须留破例通道。这三条换任何项目都成立。
- 一次性偏好：具体选了「色相」和「投影」这两条，是因为这两天用户纠正最多的就是它们。换个项目该查什么得重新看。
- 悬而未决：字号阶梯什么时候纳入 CI，取决于存量什么时候收敛。现在定时间表没意义。

## 12. CI 强制检查

前面所有章节靠的是「读了会遵守」。`CLAUDE.md` / `AGENTS.md` / `.cursor/rules` / `.github/copilot-instructions.md` 四个入口能保证规范被**加载**，但保证不了写代码时**遵守**。CI 是唯一会亮红灯的一层。

**脚本**：`scripts/design-check.mjs`，本地 `npm run design-check`，CI 在 PR 上自动跑（`.github/workflows/design-check.yml`）。

**只查两条，只查本次改动新增的 CSS 行**：

1. 引入了灰阶以外的新色相（`--accent` #7462bd 例外；平台 / 合作方 logo 走图片资产，不写进 CSS）
2. 新增投影（`inset` 的 1px 描边不算；§6 批准的卡片 hover `0 8px 28px rgba(25,25,25,.06)` 例外）

**为什么只查新增行**：仓库现存 94 种 hex、11 处投影、38 种字号。全量检查会让 main 直接全红，那种检查一周内就会被关掉。存量靠平时顺手收敛。

**为什么没做字号检查**：38 种现状意味着几乎每次改动都会撞线。一个天天误报的检查等于没有检查——大家会先学会无视它，再顺手无视其他红灯。等存量收敛到十几种以内再加。

**破例**：在那一行加 `/* design-check-ignore */`，并在 PR 里说明理由。破例要留痕，但不禁止破例——真有活动页需要一抹彩色时，红灯不该变成办不成事的理由。

**这层拦不住什么**：板块结构、留白节奏、文案语气、头尾有没有真的复用。这些机器判不准，硬查只会误报。它们仍然靠 §9 自查和人工 review。
