# astro-whono

[中文](README.md) | [English](README.en.md)

[![CI](https://img.shields.io/github/actions/workflow/status/cxro/astro-whono/ci.yml?style=flat&label=CI&labelColor=2E3440&color=A3BE8C&logo=githubactions&logoColor=ECEFF4)](https://github.com/cxro/astro-whono/actions/workflows/ci.yml)  [![Node](https://img.shields.io/badge/Node-%3E%3D22.12.0-81A1C1?style=flat&labelColor=2E3440&logo=nodedotjs&logoColor=ECEFF4)](https://github.com/cxro/astro-whono#%E7%8E%AF%E5%A2%83%E8%A6%81%E6%B1%82)  [![Astro](https://img.shields.io/github/package-json/dependency-version/cxro/astro-whono/astro?branch=main&style=flat&label=Astro&labelColor=2E3440&color=BC52EE&logo=astro&logoColor=ECEFF4)](https://docs.astro.build/)  [![License](https://img.shields.io/badge/License-MIT-4C566A?style=flat&labelColor=2E3440&logo=opensourceinitiative&logoColor=ECEFF4)](LICENSE)

**✨ astro-whono 支持本地后台可视化预览写作**

一个极简双栏的 Astro 主题，用于个人写作与轻量内容发布。


## 链接

- 在线演示：<https://astro.whono.me>
- 仓库地址：<https://github.com/cxro/astro-whono>


## 预览

<p align="center">
  <img src="public/preview-light.png" width="49%" alt="浅色预览" />
  <img src="public/preview-dark.png" width="49%" alt="深色预览" />
</p>


## 特性

- 双栏布局（侧栏导航 + 内容区）
- 移动端适配
- 内容集合：随笔 / 絮语 / 小记 / 关于（归档为目录视图）
- 内置本地 Admin Console（/admin）：开发环境下管理站点配置、内容与图片资源
- 絮语草稿生成器：/bits 页面一键生成 Markdown（复制/下载），支持多图与自动读取尺寸
- RSS：默认归档订阅 + 分栏订阅
- 浅色 / 深色模式 + 阅读模式


## 开始使用

### 环境要求

- Node.js 22.12+（建议使用 `.nvmrc`）


### 快速开始

```bash
npm install
npm run dev
```

<details>
  <summary>Windows（PowerShell）提示</summary>

如遇执行策略拦截 `npm.ps1`，可用：

- `cmd /c npm run ...`
- 或改用 Git Bash / WSL
</details>


### 常用命令

  - `npm run dev`：启动本地开发服务
  - `npm run build`：生成静态站点
  - `npm run preview`：预览构建产物
  - `npm run new:bit`：创建一条 bits 草稿

<details>
  <summary>维护者校验</summary>

以下命令用于维护主题本身，普通写作与部署通常不需要执行。

```bash
# 基础回归：Astro check、Vitest、build
npm run verify

# Markdown 渲染契约：改动渲染链路、文章样式或代码块工具栏时执行
npm run build
npm run check:markdown-smoke

# 发布前产物检查：需已确定正式域名
SITE_URL=https://你的域名 npm run build
SITE_URL=https://你的域名 npm run check:prod-artifacts

# Admin 边界检查：仅改动 /admin/** 或 /api/admin/** 时执行
npm run check:preview-admin

# 生产依赖审计：发布前或依赖变更时执行
npm run audit:prod
```
</details>


## 部署

### 一键部署

[![Deploy to Vercel](https://img.shields.io/badge/Deploy-Vercel-000000?style=flat&logo=vercel&logoColor=white)](https://vercel.com/new/clone?repository-url=https://github.com/cxro/astro-whono)&nbsp;&nbsp;[![Deploy to Netlify](https://img.shields.io/badge/Deploy-Netlify-00C7B7?style=flat&logo=netlify&logoColor=white)](https://app.netlify.com/start/deploy?repository=https://github.com/cxro/astro-whono)&nbsp;&nbsp;[![Deploy to Cloudflare Pages](https://img.shields.io/badge/Deploy-Cloudflare%20Pages-F38020?style=flat&logo=cloudflare&logoColor=white)](https://dash.cloudflare.com/?to=/:account/workers-and-pages)

> 建议在生产环境设置：SITE_URL=https://你的域名 （不要以 / 结尾）。
> 未设置时会使用占位地址，页面可访问，但分享与收录相关链接可能不完整。

<details>
  <summary><strong>Cloudflare Pages 部署（手动导入仓库）</strong></summary>

**构建设置**
- Framework preset：Astro
- Build command：`npm run build`
- Output directory：`dist`

**Node.js 版本（通常不用填）**
- 本项目已提供 `.nvmrc`，Cloudflare Pages 会自动读取。
- 如需手动指定，可在 Pages 的环境变量里设置：`NODE_VERSION=22.22.0`

**环境变量（生产环境应设置）**
- 在 Pages 项目 → Settings → Environment variables 添加：`SITE_URL=https://你的域名`（例如 `https://astro.whono.me`，不要以 `/` 结尾）
- `SITE_URL` 用于生成 canonical、Open Graph 的 `og:url`、RSS 链接与 sitemap 等绝对链接；未设置时相关链接会退化为占位域名，影响分享预览与搜索收录。

**关于 sitemap / robots**
- 只有设置了 `SITE_URL`，才会生成 sitemap，并且 `/robots.txt` 才会输出 `Sitemap:` 行（避免指向错误域名）。

</details>

<details>
<summary><strong>部署后检查</strong></summary>

- 首页 / 列表 / 详情页可访问
- RSS 可访问（`/rss.xml` 及分栏 RSS）
- 设置 `SITE_URL` 后：canonical / `og:url` 指向你的域名
- Network 不再请求演示域名资源

</details>


## 配置与入口

### 项目入口

- 站点配置：`site.config.mjs`
- 内容集合：`src/content.config.ts`
- 样式共享入口：`src/styles/global.css`
- 页面 / 场景样式入口：`src/styles/home.css`、`src/styles/about.css`、`src/styles/memo.css`、`src/styles/article.css`、`src/styles/bits-page.css`
- 后台样式入口：`src/styles/components/admin/shell.css` + `src/styles/components/admin/**` 路由私有样式；不再提供全量 `admin.css` 聚合入口

### Admin Console（/admin）

内置本地 Admin Console，仅面向开发环境，用于查看站点概况、调整主题配置、编辑内容与导入导出 settings 快照。

启动 `npm run dev` 后访问 `http://localhost:4321/admin/`（端口以实际为准）。

| 入口 | 用途 |
| :---: | :--- |
| `/admin/` | 后台稳定入口与 Site Overview |
| `/admin/theme/` | Theme Console，编辑站点信息、侧栏、首页与内页文案等 |
| `/admin/images/` | 图片资源浏览与路径辅助 |
| `/admin/data/` | settings 快照导出 / dry-run 导入 / 确认写入 |
| `/admin/content/` | 随笔 / 絮语 / 小记 / 关于页的本地编辑、新建与源文件导出 |

> 使用详情：[Admin Console 快速指南](https://astro.whono.me/archive/admin-console-guide/) · [Theme Console 配置指南](https://astro.whono.me/archive/theme-console-guide/) · [Content Console 使用指南](https://astro.whono.me/archive/content-console-guide/)

生产构建保持静态站点输出：`/admin/` 可按 Theme 设置显示只读公开 Overview 或关闭态文案，其他后台子路由与 `/api/admin/**` 仅在本地开发可用。

#### 兼容迁移（已 fork 用户）

- 未创建 `src/data/settings/*.json` 时，前台仍按 `settings > legacy > default` 读取
- 首次在 `/admin/theme/` 保存后才会生成对应的 JSON 文件，无需手动迁移


## 内容与写作

内容集合、源文件与公开入口如下，字段和写作细节见现有指南：

| 类型 | 源文件 | 主要入口 |
| --- | --- | --- |
| 随笔 | `src/content/essay/` | `/essay/`、`/archive/`、`/archive/[slug]/` |
| 絮语 | `src/content/bits/` | `/bits/` |
| 小记 | `src/content/memo/index.md` | `/memo/` |
| 关于 | `src/content/about/index.md` | `/about/` |

- `essay` / `bits` 是多条内容；`memo` / `about` 是固定单页。
- `essay` / `bits` 的 `draft: true` 只在本地开发中用于预览，生产构建、公开列表和 RSS 会过滤；`memo` 不应标记为草稿。
- `essay.archive: false` 只退出 `/archive/` 聚合与归档 RSS，不代表隐藏文章；详情页、`/essay/` 和 essay RSS 仍可见。
- Admin Console 图片上传默认保存到本地，也可在开发环境配置 S3 兼容对象存储。Images Console 可浏览云端图片并复制 URL。
- 图片上传、frontmatter 字段、日期与摘要规则见 [Content Console 使用指南](https://astro.whono.me/archive/content-console-guide/)；Callout、Figure、Gallery、公式等 Markdown 扩展见 [Markdown 排版指南](https://astro.whono.me/archive/markdown-guide/)。


## 字体与许可

本主题使用两套字体排版（自托管 + 子集化）：
- Noto Serif SC（400 / 600）
- LXGW WenKai Lite（Regular）

仓库提交的是子集化后的 WOFF2 字体（latin / cjk-common / cjk-ext 三段，`unicode-range` 按需加载），因此 **clone 即用**。
子集字符集由仓库文本 + `tools/charset-base.txt`（3500 常用字）共同生成，用来降低缺字概率。

缺字或更换源字体时，运行 `npm run font:build` 重新生成子集；步骤与文件清单见下。

<details>
  <summary>子集再生成与文件清单</summary>

1. 安装 Python 3，执行 `python -m pip install fonttools brotli zopfli`，确认 `pyftsubset --help` 可用（不可用时把 Python Scripts 目录加入 `PATH`）
2. 把源字体放到 `tools/fonts-src/`
3. 运行 `npm run font:build`；缺字时把字符补到 `tools/charset-base.txt` 后重跑
4. `tools/charset-common.txt` 由 `npm run font:charset` 重生成，不要手改

子集文件（仓库内）：
- `public/fonts/lxgw-wenkai-lite-latin.woff2`
- `public/fonts/lxgw-wenkai-lite-cjk-common.woff2`
- `public/fonts/lxgw-wenkai-lite-cjk-ext.woff2`
- `public/fonts/noto-serif-sc-400-latin.woff2`
- `public/fonts/noto-serif-sc-400-cjk-common.woff2`
- `public/fonts/noto-serif-sc-400-cjk-ext.woff2`
- `public/fonts/noto-serif-sc-600-latin.woff2`
- `public/fonts/noto-serif-sc-600-cjk-common.woff2`
- `public/fonts/noto-serif-sc-600-cjk-ext.woff2`

源字体（不入库）：
- `tools/fonts-src/LXGWWenKaiLite-Regular.woff2`
- `tools/fonts-src/NotoSerifSC-Regular.ttf`
- `tools/fonts-src/NotoSerifSC-SemiBold.ttf`
</details>

字体许可：SIL Open Font License 1.1（见 `public/fonts/OFL-LXGW-WenKai-Lite.txt` 与 `public/fonts/OFL-NotoSerifSC.txt`）。

### 配置排版字体

在开发模式下打开 Theme Console（`/admin/theme/` →「排版字体」），可分别设置正文、文案、等宽和品牌字体，保存后在下次构建时生效。选项包括系统字体、自托管字体，以及构建时下载并自托管的在线字体，页面加载不访问第三方字体服务；内置选项以外的字体在 `src/lib/fonts/registry.ts` 中注册。详见 [Theme Console 配置指南 →「排版字体」](https://astro.whono.me/archive/theme-console-guide/)。

运行 `npm run check:font-charset` 可检查字符集和字体子集是否与站点内容一致；检查失败时，按提示运行 `npm run font:build` 重新生成。

### 配置站点图标

在开发模式下打开 Theme Console（`/admin/theme/` →「站点设置」→「站点图标」），可上传正方形 PNG 分别自定义浏览器标签页图标与移动端触摸图标。上传文件以内容哈希命名写入 `public/images/site/`，替换后不受浏览器图标缓存影响；保存并重新构建后生效。

自定义标签页图标（SVG 或 PNG 任一）后，另一空槽位不再输出主题默认图标，避免部分浏览器继续显示默认图标；触摸图标独立回退，未自定义时保持主题默认。SVG 图标暂不支持在控制台上传，可直接替换 `public/favicon.svg`，或在 `src/data/settings/site.json` 的 `favicon.svg` 中填写 `public/**` 下的 SVG 路径。


## RSS

- `/rss.xml`（默认 RSS；与 `/archive/rss.xml` 使用同源归档数据）
- `/archive/rss.xml`（归档订阅）
- `/essay/rss.xml`

部署时建议设置 `SITE_URL`（影响 RSS/OG/canonical 的绝对链接）。


## 贡献

欢迎创建 Issue 来报告问题或提出想法。
欢迎提交 Pull Request 参与开发，建议从 feature/* 分支发起。

### Fork 同步上游

```bash
git remote add upstream https://github.com/cxro/astro-whono.git
git fetch upstream --tags
git checkout main
git merge upstream/main
git push origin main --tags
```


## 致谢

- 感谢 [elizen/elizen-blog](https://github.com/elizen/elizen-blog)，这是本主题设计的起点，其风格源自Hugo 主题  [yihui/hugo-ivy](https://github.com/yihui/hugo-ivy)


## 许可证

License：MIT


## 🌐 Web Resources & Aesthetic Symbols Index
- [SYM 1D47F](https://lace-heart-kaomoji-64.pages.dev/symbol/sym-1d47f/)
- [SYM 1D496](https://sleek-line-symbols-51.pages.dev/symbol/sym-1d496/)
- [SYM 2679](https://theeduplaycampen.pages.dev/symbol/sym-2679/)
- [SYM 1D48F](https://matrix-glitch-text-37.pages.dev/symbol/sym-1d48f/)
- [SYM 2675](https://futuristic-gaming-fonts-52.pages.dev/symbol/sym-2675/)
- [NATURE FLOWERS](https://chibi-emoticon-lab-65.pages.dev/vi/nature-flowers/)
- [SYM 2615](https://minimal-star-symbols-93.pages.dev/symbol/sym-2615/)
- [SYM 1D444](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d444/)
- [FREEFIRE NAMES](https://glitch-font-studio-46.pages.dev/freefire-names/)
- [SYM 1D43A](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d43a/)
- [SYM 1F924](https://scholarly-vintage-symbols-48.pages.dev/symbol/sym-1f924/)
- [SYM 2677](https://sleek-line-symbols-51.pages.dev/symbol/sym-2677/)
- [SYM 2684](https://sleek-line-symbols-51.pages.dev/symbol/sym-2684/)
- [SYM 1D47C](https://techwear-bio-symbols-45.pages.dev/symbol/sym-1d47c/)
- [SYM 1D475](https://neon-glitch-fonts-20.pages.dev/symbol/sym-1d475/)
- [SYM 2637](https://anime-sparkle-text-73.pages.dev/symbol/sym-2637/)
- [SYM 1F498](https://anime-sparkle-text-73.pages.dev/symbol/sym-1f498/)
- [SYM 1D462](https://coquette-aesthetic-symbols-52.pages.dev/symbol/sym-1d462/)
- [SYM 260D](https://cyber-clan-tags-75.pages.dev/symbol/sym-260d/)
- [SYM 1D462](https://kawaii-kaomoji-hub-31.pages.dev/symbol/sym-1d462/)
- [SYM 1D46C](https://neon-matrix-symbols-94.pages.dev/symbol/sym-1d46c/)
- [SYM 1F629](https://minimal-star-symbols-25.pages.dev/symbol/sym-1f629/)
- [MUSIC WEATHER](https://sleek-arrow-symbols-42.pages.dev/music-weather/)
- [SYM 1D418](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d418/)
- [NATURE FLOWERS](https://neon-glitch-symbols-84.pages.dev/es/nature-flowers/)
- [INSTAGRAM BIO](https://theeduplaycampen.pages.dev/ja/instagram-bio/)
- [VI](https://mecha-synth-kaomoji-92.pages.dev/vi/)
- [TRENDING](https://anime-sparkle-text-81.pages.dev/pt/trending/)
- [TRENDING](https://kawaii-kaomoji-hub-77.pages.dev/ru/trending/)
- [SYM 2633](https://ribbon-bow-unicode-18.pages.dev/symbol/sym-2633/)
- [SYM 1D440](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d440/)
- [EIGHT POINTED BLACK STAR](https://minimal-star-symbols-25.pages.dev/symbol/eight-pointed-black-star/)
- [SYM 1F618](https://pastel-manga-symbols-57.pages.dev/symbol/sym-1f618/)
- [SYM 1D45B](https://glitch-font-studio-46.pages.dev/symbol/sym-1d45b/)
- [GREEK PSI TRIDENT](https://futuristic-gaming-fonts-52.pages.dev/symbol/greek-psi-trident/)
- [SYM 1D4A0](https://cyber-clan-tags-90.pages.dev/symbol/sym-1d4a0/)
- [SYM 1D451](https://alchemist-symbol-hub-29.pages.dev/symbol/sym-1d451/)
- [SYM 262F](https://anime-sparkle-text-73.pages.dev/symbol/sym-262f/)
- [SYM 26EA](https://lace-heart-kaomoji-64.pages.dev/symbol/sym-26ea/)
- [SYM 2672](https://mecha-synth-kaomoji-92.pages.dev/symbol/sym-2672/)
- [SYM 2611](https://kawaii-kaomoji-hub-77.pages.dev/symbol/sym-2611/)
- [SYM 1D453](https://alchemist-symbol-hub-29.pages.dev/symbol/sym-1d453/)
- [ANTICLOCKWISE OPEN CIRCLE ARROW](https://minimal-star-symbols-43.pages.dev/symbol/anticlockwise-open-circle-arrow/)
- [SYM 2677](https://anime-sparkle-text-23.pages.dev/symbol/sym-2677/)
- [SYM 2643](https://vintage-angel-text-38.pages.dev/symbol/sym-2643/)
- [ROBLOX NAMES](https://daintystar-font-studio-48.pages.dev/vi/roblox-names/)
- [RIGHTWARDS PAIRED HARPOON](https://futuristic-gaming-fonts-52.pages.dev/symbol/rightwards-paired-harpoon/)
- [SYM 260F](https://vintage-angel-text-38.pages.dev/symbol/sym-260f/)
- [SYM 2746](https://glitch-font-studio-46.pages.dev/symbol/sym-2746/)
- [SYM 2724](https://synthwave-text-vault-95.pages.dev/symbol/sym-2724/)
- [SYM 1F47B](https://chibi-emoticon-lab-65.pages.dev/symbol/sym-1f47b/)
- [MUSIC WEATHER](https://pastel-manga-symbols-57.pages.dev/pt/music-weather/)
- [SYM 1FA77](https://chibi-emoticon-lab-65.pages.dev/symbol/sym-1fa77/)
- [SYM 1D497](https://pearl-heart-symbols-95.pages.dev/symbol/sym-1d497/)
- [SYM 1F480](https://minimal-star-symbols-43.pages.dev/symbol/sym-1f480/)
- [SYM 1D432](https://synthwave-text-vault-95.pages.dev/symbol/sym-1d432/)
- [WHITE FLORETTE BLOSSOM](https://kawaii-kaomoji-hub-77.pages.dev/symbol/white-florette-blossom/)
- [SYM 1D494](https://aesthetic-spacing-fonts-10.pages.dev/symbol/sym-1d494/)
- [SYM 1D453](https://sleek-arrow-symbols-42.pages.dev/symbol/sym-1d453/)
- [SYM 1F49A](https://anime-sparkle-text-73.pages.dev/symbol/sym-1f49a/)
- [SYM 1F63D](https://synthwave-text-vault-95.pages.dev/symbol/sym-1f63d/)
- [SYM 263A FE0F](https://theeduplaycampen.pages.dev/symbol/sym-263a-fe0f/)
- [GAMING WEAPONS](https://monochrome-text-lab-86.pages.dev/gaming-weapons/)
- [FREEFIRE NAMES](https://sleek-bio-symbols-51.pages.dev/freefire-names/)
- [SYM 1D421](https://fairy-lace-symbols-92.pages.dev/symbol/sym-1d421/)
- [SYM 1D41B](https://matrix-glitch-text-37.pages.dev/symbol/sym-1d41b/)
- [SYM 1F649](https://cyber-clan-tags-75.pages.dev/symbol/sym-1f649/)
- [SYM 1D40A](https://pearl-heart-symbols-95.pages.dev/symbol/sym-1d40a/)
- [FLUTTERING BUTTERFLY](https://pastel-manga-symbols-57.pages.dev/symbol/fluttering-butterfly/)
- [SWIMMING FISH LEFT](https://futuristic-gaming-fonts-52.pages.dev/symbol/swimming-fish-left/)
- [TIKTOK CAPTIONS](https://anime-sparkle-text-23.pages.dev/es/tiktok-captions/)
- [SYM 26CA](https://gothic-bio-fonts-81.pages.dev/symbol/sym-26ca/)
- [SYM 268D](https://futuristic-gaming-fonts-52.pages.dev/symbol/sym-268d/)
- [GAMING WEAPONS](https://pastel-manga-symbols-57.pages.dev/ru/gaming-weapons/)
- [SYM 1D41B](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d41b/)
- [SYM 2742](https://chibi-emoticon-lab-65.pages.dev/symbol/sym-2742/)
- [DISCORD STATUS](https://daintystar-font-studio-48.pages.dev/ru/discord-status/)
- [ROBLOX NAMES](https://zen-unicode-symbols-89.pages.dev/roblox-names/)
- [SYM 1F92F](https://vintage-angel-text-38.pages.dev/symbol/sym-1f92f/)
- [SYM 1D423](https://futuristic-gaming-fonts-52.pages.dev/symbol/sym-1d423/)
- [SYM 26E2](https://kawaii-kaomoji-hub-93.pages.dev/symbol/sym-26e2/)
- [SYM 26EC](https://mecha-gamer-fonts-53.pages.dev/symbol/sym-26ec/)
- [SYM 1D469](https://futuristic-gaming-fonts-52.pages.dev/symbol/sym-1d469/)
- [SYM 1F62D](https://sleek-bio-symbols-51.pages.dev/symbol/sym-1f62d/)
- [SYM 1D492](https://gothic-bio-fonts-81.pages.dev/symbol/sym-1d492/)
- [SYM 1D46B](https://mecha-gamer-fonts-53.pages.dev/symbol/sym-1d46b/)
- [SYM 1F613](https://vintage-angel-text-38.pages.dev/symbol/sym-1f613/)
- [SYM 263F](https://synthwave-text-vault-95.pages.dev/symbol/sym-263f/)
- [SYM 2670](https://alchemist-symbol-hub-29.pages.dev/symbol/sym-2670/)
- [SYM 26A6](https://vintage-angel-text-38.pages.dev/symbol/sym-26a6/)
- [SYM 2677](https://pearl-heart-symbols-95.pages.dev/symbol/sym-2677/)
- [NATURE FLOWERS](https://theeduplaycampen.pages.dev/ru/nature-flowers/)
- [SYM 1D40A](https://glitch-font-studio-46.pages.dev/symbol/sym-1d40a/)
- [SYM 1F47D](https://matrix-hacker-text-52.pages.dev/symbol/sym-1f47d/)
- [SAGITTARIUS ZODIAC ARCHER](https://daintystar-font-studio-48.pages.dev/symbol/sagittarius-zodiac-archer/)
- [SYM 1F623](https://vintage-angel-text-38.pages.dev/symbol/sym-1f623/)
- [SYM 1D414](https://chibi-emoticon-lab-65.pages.dev/symbol/sym-1d414/)
- [FREE FIRE CLAN EMPEROR CROWN](https://mecha-synth-kaomoji-92.pages.dev/symbol/free-fire-clan-emperor-crown/)
- [SYM 1F608](https://vintage-angel-text-38.pages.dev/symbol/sym-1f608/)
- [UPWARD DIAGONAL ARROW](https://coquette-aesthetic-symbols-62.pages.dev/symbol/upward-diagonal-arrow/)
- [BLACK FLORETTE FLOWER](https://minimal-star-symbols-25.pages.dev/symbol/black-florette-flower/)
- [SYM 1D458](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d458/)
- [SYM 1D46B](https://pearl-girly-fonts-86.pages.dev/symbol/sym-1d46b/)
- [SYM 1F602](https://mecha-gamer-fonts-53.pages.dev/symbol/sym-1f602/)
- [SYM 2626](https://sleek-arrow-symbols-42.pages.dev/symbol/sym-2626/)
- [SYM 26E8](https://futuristic-gaming-fonts-52.pages.dev/symbol/sym-26e8/)
- [ES](https://pastel-moe-emoticons-80.pages.dev/es/)
- [SYM 2663](https://alchemist-symbol-hub-29.pages.dev/symbol/sym-2663/)
- [FREEFIRE NAMES](https://daintystar-font-studio-48.pages.dev/ru/freefire-names/)
- [HOLLOW STAR](https://sleek-line-symbols-51.pages.dev/symbol/hollow-star/)
- [SYM 1D442](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d442/)
- [TAURUS ZODIAC BULL](https://alchemist-symbol-hub-29.pages.dev/symbol/taurus-zodiac-bull/)
- [SYM 2655](https://sleek-line-symbols-51.pages.dev/symbol/sym-2655/)
- [SYM 1D447](https://futuristic-gaming-fonts-52.pages.dev/symbol/sym-1d447/)
- [SYM 267C](https://alchemist-symbol-hub-29.pages.dev/symbol/sym-267c/)
- [NATURE FLOWERS](https://pastel-manga-symbols-57.pages.dev/nature-flowers/)
- [ZODIAC CELESTIAL](https://pastel-manga-symbols-57.pages.dev/ru/zodiac-celestial/)
- [SYM 1D42C](https://glitch-font-studio-46.pages.dev/symbol/sym-1d42c/)
- [SYM 1F640](https://synthwave-text-vault-95.pages.dev/symbol/sym-1f640/)
- [SYM 263A FE0F](https://sleek-bio-symbols-51.pages.dev/symbol/sym-263a-fe0f/)
- [SYM 1D425](https://sleek-line-symbols-51.pages.dev/symbol/sym-1d425/)
- [SYM 26EB](https://coquette-symbols.pages.dev/symbol/sym-26eb/)
- [SYM 2647](https://minimal-star-symbols-43.pages.dev/symbol/sym-2647/)
- [SYM 1F644](https://occult-rune-symbols-64.pages.dev/symbol/sym-1f644/)
- [SYM 1F48C](https://clean-aesthetic-fonts-73.pages.dev/symbol/sym-1f48c/)
- [SYM 26E8](https://neon-glitch-symbols-84.pages.dev/symbol/sym-26e8/)
- [HIGH VOLTAGE LIGHTNING](https://cyber-clan-tags-90.pages.dev/symbol/high-voltage-lightning/)
- [SYM 260E](https://pearl-heart-symbols-95.pages.dev/symbol/sym-260e/)
- [SYM 2645](https://mecha-synth-kaomoji-92.pages.dev/symbol/sym-2645/)
- [SYM 1D44D](https://pastel-moe-emoticons-55.pages.dev/symbol/sym-1d44d/)
