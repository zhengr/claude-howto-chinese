<picture>
  <source media="(prefers-color-scheme: dark)" srcset="logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="logos/claude-howto-logo.svg">
</picture>

# Claude How To - 品牌资源

Claude How To 项目的完整标志、图标和 favicon 资源集合。所有资源均采用 V3.0 设计：一个带有代码括号 (`>`) 符号的指南针，代表对代码导航的引导——使用黑/白/灰配色方案和亮绿色 (#22C55E) 作为强调色。

## 目录结构

```
resources/
├── logos/
│   ├── claude-howto-logo.svg       # 主标志 - 亮色模式 (520×120px)
│   └── claude-howto-logo-dark.svg  # 主标志 - 暗色模式 (520×120px)
├── icons/
│   ├── claude-howto-icon.svg       # 应用图标 - 亮色模式 (256×256px)
│   └── claude-howto-icon-dark.svg  # 应用图标 - 暗色模式 (256×256px)
└── favicons/
    ├── favicon-16.svg              # 小图标 - 16×16px
    ├── favicon-32.svg              # 小图标 - 32×32px (主)
    ├── favicon-64.svg              # 小图标 - 64×64px
    ├── favicon-128.svg             # 小图标 - 128×128px
    └── favicon-256.svg             # 小图标 - 256×256px
```

额外资源位于 `assets/logo/`：
```
assets/logo/
├── logo-full.svg       # 标记 + 文字标记（水平布局）
├── logo-mark.svg       # 仅指南针符号 (120×120px)
├── logo-wordmark.svg   # 仅文字
├── logo-icon.svg       # 应用图标 (512×512, 圆角)
├── favicon.svg         # 16×16 优化版
├── logo-white.svg      # 深色背景白色版本
└── logo-black.svg      # 黑色单色版本
```

## 资源概览

### 设计理念 (V3.0)

**指南针与代码括号** — 指引与代码的相遇：
- **指南针环** = 导航，找到你的方向
- **北针（绿色）** = 方向，学习路径上的进步
- **南针（黑色）** = 稳固，坚实的基础
- **`>` 括号** = 终端提示符、代码、CLI 上下文
- **刻度标记** = 精确性、结构化学习

### 标志

**文件**：
- `logos/claude-howto-logo.svg`（亮色模式）
- `logos/claude-howto-logo-dark.svg`（暗色模式）

**规格**：
- **尺寸**：520×120 像素
- **用途**：带有文字标记的主页眉/品牌标志
- **使用场景**：
  - 网站页眉
  - README 徽章
  - 营销材料
  - 印刷材料
- **格式**：SVG（完全可缩放）
- **模式**：亮色（白色背景）和暗色（#0A0A0A 背景）

### 图标

**文件**：
- `icons/claude-howto-icon.svg`（亮色模式）
- `icons/claude-howto-icon-dark.svg`（暗色模式）

**规格**：
- **尺寸**：256×256 像素
- **用途**：应用图标、头像、缩略图
- **使用场景**：
  - 应用图标
  - 用户头像
  - 社交媒体缩略图
  - 文档页眉
- **格式**：SVG（完全可缩放）
- **模式**：亮色（白色背景）和暗色（#0A0A0A 背景）

**设计元素**：
- 带有基点和间基点刻度标记的指南针环
- 绿色北针（方向/指引）
- 黑色南针（基础）
- 中心的 `>` 代码括号（终端/CLI）
- 绿色中心圆点强调

### Favicons

针对 Web 使用的多种尺寸的优化版本：

| 文件 | 尺寸 | 分辨率 | 用途 |
|------|------|-----|-------|
| `favicon-16.svg` | 16×16 像素 | 1x | 浏览器标签页（旧版浏览器） |
| `favicon-32.svg` | 32×32 像素 | 1x | 标准浏览器 favicon |
| `favicon-64.svg` | 64×64 像素 | 1x-2x | 高分辨率显示屏 |
| `favicon-128.svg` | 128×128 像素 | 2x | Apple 触控图标、书签 |
| `favicon-256.svg` | 256×256 像素 | 4x | 现代浏览器、PWA 图标 |

**优化说明**：
- 16px：最小几何形状——仅环、针和 V 形标记
- 32px：添加基点刻度标记
- 64px+：完整细节，包含间基点刻度
- 所有版本都与主图标保持视觉一致性
- SVG 格式确保在任何尺寸下都清晰显示

## HTML 集成

### 基本 Favicon 设置

```html
<!-- 浏览器 favicon -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

<!-- Apple 触控图标（移动设备主屏幕） -->
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

<!-- PWA 和现代浏览器 -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
```

### 完整设置

```html
<head>
  <!-- 主 favicon -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

  <!-- Apple 触控图标 -->
  <link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

  <!-- PWA 图标 -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">

  <!-- Android -->
  <link rel="shortcut icon" href="/resources/favicons/favicon-256.svg">

  <!-- PWA manifest 引用（如果使用 manifest.json） -->
  <meta name="theme-color" content="#000000">
</head>
```

## 配色方案

### 主色
- **黑色**：`#000000`（主要文字、描边、南针）
- **白色**：`#FFFFFF`（亮色背景）
- **灰色**：`#6B7280`（次要文字、次要刻度标记）

### 强调色
- **亮绿色**：`#22C55E`（北针、中心圆点、强调线——仅用于高亮，不作背景色）

### 暗色模式
- **背景**：`#0A0A0A`（近黑色）

### CSS 变量
```css
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```

### Tailwind 配置
```js
colors: {
  brand: {
    primary: '#000000',
    secondary: '#6B7280',
    accent: '#22C55E',
  }
}
```

### 使用指南
- 使用黑色作为主要文字和结构元素
- 使用灰色作为次要/辅助元素
- 绿色**仅**用于高亮——针、圆点、强调线
- 切勿将绿色用作背景色
- 保持 WCAG AA 对比度（最低 4.5:1）

## 设计指南

### 标志使用
- 在白色或深色（#0A0A0A）背景上使用
- 按比例缩放
- 在标志周围包含留白空间（最小：标志高度 / 2）
- 对相应的背景使用提供的亮色/暗色版本

### 图标使用
- 使用标准尺寸：16、32、64、128、256 像素
- 保持指南针比例
- 按比例缩放

### Favicon 使用
- 根据上下文使用适当的尺寸
- 16-32px：浏览器标签页、书签
- 64px：网站图标
- 128px+：Apple/Android 主屏幕

## SVG 优化

所有 SVG 文件均为扁平化设计，无渐变或滤镜：
- 干净的基于描边的几何形状
- 无嵌入式栅格图
- 优化的路径
- 响应式 viewBox

用于 Web 优化：
```bash
# 压缩 SVG 同时保持质量
svgo --config='{
  "js2svg": {
    "indent": 2
  },
  "plugins": [
    "convertStyleToAttrs",
    "removeRasterImages"
  ]
}' input.svg -o output.svg
```

## PNG 转换

将 SVG 转换为 PNG 以支持旧版浏览器：

```bash
# 使用 ImageMagick
convert -density 300 -background none favicon-256.svg favicon-256.png

# 使用 Inkscape
inkscape -D -z --file=favicon-256.svg --export-png=favicon-256.png
```

## 无障碍性

- 高对比度颜色比率（符合 WCAG AA 标准——最低 4.5:1）
- 干净的几何形状在各个尺寸都可识别
- 可缩放的矢量格式
- 图标中不含文字（文字在文字标记中单独添加）
- 无红绿色依赖来表达含义

## 归属

这些资源是 Claude How To 项目的一部分。

**许可证**：MIT（参见项目 LICENSE 文件）

## 版本历史

- **v3.0**（2026 年 2 月）：指南针-括号设计，黑/白/灰 + 绿色强调配色
- **v2.0**（2026 年 1 月）：受 Claude 启发的 12 射线星爆设计，翡翠色配色
- **v1.0**（2026 年 1 月）：基于六边形的渐进式图标原始设计

---

**最后更新**：2026 年 2 月
**当前版本**：3.0（指南针-括号）
**所有资源**：生产就绪的 SVG，完全可缩放，符合 WCAG AA 无障碍标准
