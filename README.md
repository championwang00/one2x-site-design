# one2x-site-design

One2X 企业视频服务官网（[enterprise.one2x.ai](https://enterprise.one2x.ai/)）的页面设计规范，打包成一个 agent skill。

给这个官网新增页面、板块、落地页或案例页时先读它，能保证新页面和现有首页是同一套东西：统一的 header / footer、设计 token、板块语法和组件库。

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
- 自动触发：说「按官网风格」「和官网保持一致」会命中

## 关于这个仓库

这是一份**公开镜像**。唯一真源在 `one2x-ai/one2x-enterprise` 仓库的 `.claude/skills/one2x-site-design/SKILL.md`，改动先落在那边，再同步到这里。两处不一致时以源仓库为准。
