# 头像玄学 Crush 分析器

一款趣味性 Web 工具：用户上传暗恋对象头像并选择性别，通过「玄学」分析生成命宫标签、性格解析和追求建议。星空主题 + 丰富动画。

从 `https://app-d6o22l5w3hmp.appmiaoda.com` 下载的原始站点，可作为二次开发的基础。

## 目录结构

```
crush-analyzer/
├── index.html        # 主页面（CSS / JS 均内联，单文件应用）
├── assets/
│   └── favicon.png   # 站点头像/图标
└── README.md
```

## 本地运行

无需构建，直接起一个静态服务器即可：

```bash
cd crush-analyzer
python3 -m http.server 8000
```

浏览器打开 http://localhost:8000

> 直接双击 `index.html` 也能打开，但部分浏览器对 `file://` 下资源加载有限制，建议用上面的本地服务器方式。

## 代码结构说明

`index.html` 内部主要分为四块（行号为大致位置）：

- `<style>`（第 24–963 行）：全部页面样式，CSS 变量集中在 `:root`，主题为深紫星空 + 金色点缀。
- 页面主体（第 972–1097 行）：星空背景 `canvas#starCanvas`、上传区、性别选择、分析按钮、结果卡片。
- 核心应用逻辑（第 1098–1504 行）：星空动画、图片上传/预览、玄学分析算法、结果渲染。**二次开发主要改这里。**
- 平台注入脚本（第 964–971、1505–1572 行）：由托管平台「妙搭」注入的 `redirect`、`md-click`、`iframe-onerror-catcher` 以及 `miaoda-external.js` 分享/埋点逻辑，**与应用自身功能无关**。

## 外部资源依赖

以下资源仍引用线上 CDN，需联网才能完整显示：

- Google Fonts：Playfair Display、Cormorant Garamond（`index.html` 头部 `<link>`）
- 妙搭字体：`https://miaoda-font.cdn.bcebos.com/static/font_v1/fonts-code.css`

如需完全离线，可自行下载这些字体改为本地引用。

## 建议的后续改造

- 将内联 `<style>` 拆分到 `css/style.css`、核心 JS 拆分到 `js/main.js`，便于维护。
- 移除第 964–971、1505–1572 行的平台注入脚本，让项目完全独立于托管平台。
- 当前「玄学分析」为纯前端本地算法，如需接入真实模型，可在核心逻辑处替换为接口调用。
