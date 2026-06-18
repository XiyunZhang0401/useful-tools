# useful-tools

> 纯前端浏览器本地工具合集 — 所有操作在浏览器本地完成，不上传服务器。

## Project

- **Stack**: 纯 HTML/CSS/JS，无构建工具、无后端依赖
- **Entry**: `index.html` — 工具导航首页
- **Tools**:
  - **PDF 装订坊** → `pdf-tools/index.html` (1890 行单文件 SPA)
- **Libraries** (CDN): [pdf-lib 1.17.1](https://pdf-lib.org/)、[PDF.js 3.11.174](https://mozilla.github.io/pdf.js/)、[html2canvas 1.4.1](https://html2canvas.hertzen.com/)
- **License**: MIT

## Commands

无需构建或包管理器。直接浏览器打开 `.html` 文件即可运行。

| 命令 | 操作 |
|------|------|
| `open index.html` | 启动本地导航页 |
| `open pdf-tools/index.html` | 启动 PDF 装订坊 |

开发时编辑 HTML 文件后刷新浏览器即可。

## Architecture

- **`index.html`** — 工具导航首页；列出所有工具的卡片式入口
- **`pdf-tools/index.html`** — PDF 装订坊单页应用，内联全部 CSS 和 JS
  - **侧边栏导航** — 3 个 Tab 切换（Merge / Insert / Img2PDF）
  - **Merge Tab** — 合并多个 PDF，支持页面级选择（range 输入 + 缩略图勾选）
  - **Insert Tab** — 选择一个"目标 PDF"（Base），选择"源 PDF"的页面，插入到目标指定位置
  - **Img2PDF Tab** — 多图转 PDF，支持图片排序
  - **PPT2PDF Tab** — PPT 批量转 PDF（每个文件生成独立 PDF 并自动下载），使用 [@aiden0z/pptx-renderer](https://github.com/aiden0z/pptx-renderer) ESM（动态 `import()` 懒加载）+ html2canvas 截图 + pdf-lib 嵌入图片
  - **通用组件** — Toast 通知、进度条、文件拖放（DropZone）、缩略图渲染

## Conventions

- **单文件结构** — 每个工具一个 `.html`，CSS 和 JS 全部内联，不拆分外链
- **CSS 自定义属性** — 主题色通过 `:root` 变量定义（`--paper`, `--ink`, `--accent` 等）
- **Tab 隔离** — 每个 Tab 的功能用 IIFE 包裹 `(function(){ ... })()`，变量不污染全局
- **命名风格** — JS 采用驼峰 `camelCase`；HTML class 用连字符 `kebab-case`；ID 用驼峰
  - 全局函数/变量（如 `showToast`, `switchTab`）直接声明
  - Tab 内局部函数用 `function` 声明，变量用 `var`
- **DOM 引用** — 各 Tab 顶部 `document.getElementById(...)` 批量获取 DOM 引用
- **异步** — PDF 操作使用 `async/await` + `.arrayBuffer()`
- **错误处理** — `try/catch` 包裹 PDF 解析，错误显示在 UI 而非控制台
- **缩略图** — canvas 渲染 JPEG thumbnail（scale 0.28, quality 0.6）
- **语言** — 中文 UI，英文注释极少

## Notes

（留空 — 供后续补充）
