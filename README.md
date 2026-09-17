# one2x-site-design

One2X 企业视频服务官网（[enterprise.one2x.ai](https://enterprise.one2x.ai/)）的页面设计规范，打包成一个 agent skill。

给这个官网新增页面、板块、落地页或案例页，或给它加语言、改文案时先读它，能保证新页面和现有首页是同一套东西：统一的 header / footer、设计 token、板块语法和组件库。

官网从 2026-09-17 起是多语言站（简体中文 `/`、韩语 `/ko/`）。§13 讲了文案怎么组织、URL 和语言怎么判定、语言切换器长什么样，以及**为什么中文那套字距不能直接套到别的文字上**。

## 安装

**Claude Code / Cowork**

```bash
git clone --depth 1 https://github.com/championwang00/one2x-site-design.git \
  ~/.claude/skills/one2x-site-design
```

**Codex**

```bash
git clone --depth 1 https://github.com/championwang00/one2x-site-design.git \
  ~/.codex/skills/one2x-site-design
```

**Cursor**（放进项目里）

```bash
git clone --depth 1 https://github.com/championwang00/one2x-site-design.git \
  .cursor/skills/one2x-site-design
```

不想用 git 的话，直接下载 `SKILL.md`，放成 `<skills 目录>/one2x-site-design/SKILL.md` 即可。目录名必须是 `one2x-site-design`。

## 调用

- 显式点名：`/one2x-site-design`
- 说名字：「用 one2x-site-design 帮我加一个案例页」
- 自动触发：说「按官网风格」「和官网保持一致」「加一门语言」会命中

## 关于这个仓库

这是一份**公开镜像**。唯一真源在 `one2x-ai/one2x-enterprise` 仓库的 `.claude/skills/one2x-site-design/SKILL.md`，改动先落在那边，再同步到这里。两处不一致时以源仓库为准。
