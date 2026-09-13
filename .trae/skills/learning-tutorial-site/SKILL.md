---
name: "learning-tutorial-site"
description: "基于 Jupyter Notebook 的交互式学习教程网站生成器，参考 modern-llm-notebook 模板。当用户需要创建 Notebook 形式的在线教程/课程网站时调用。"
---

# Learning Tutorial Site Generator

基于 Jupyter Notebook 的交互式学习教程网站生成器，参考 modern-llm-notebook 项目模板构建。

## 何时使用

当用户需要：
- 创建一个基于 Jupyter Notebook 的在线教程/课程网站
- 将 .ipynb 笔记本转化为可浏览的网页教程
- 需要中英双语支持的学习平台
- 构建带有代码高亮、数学公式、笔记功能的教程网站

## 技术栈

- **前端框架**: React 19 + Vite
- **样式**: Tailwind CSS 4
- **数学公式**: KaTeX
- **图标**: Lucide React
- **核心功能**: 直接渲染 .ipynb 文件，无需后端

## 项目结构

```
tutorial-site/
├── notebooks/           # 教程笔记本（.ipynb 文件）
│   ├── part1-image-processing/    # 第一部分
│   │   ├── 数字图像的获取和表示/   # 子目录（支持嵌套）
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg          # 图片资源
│   │   └── 几何变换/
│   │       └── practice.ipynb
│   └── part2-optimization-3d/     # 第二部分
├── web/                 # React/Vite 前端网站
│   ├── src/
│   │   ├── components/  # React 组件
│   │   ├── context/     # Context 状态管理
│   │   ├── data/        # 数据配置
│   │   ├── hooks/       # 自定义 Hooks
│   │   ├── styles/      # 全局样式
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
├── .github/workflows/   # GitHub Actions 部署
│   └── deploy.yml
└── README.md
```

## 嵌套目录支持

Notebook 可以放在任意深度的子目录中。系统会自动：
- 递归扫描所有 .ipynb 文件
- 使用 `{子目录路径}-{文件名}` 作为唯一 ID（路径分隔符替换为 -）
- 从 Notebook 的第一个 markdown 一级标题提取标题
- 重写 Markdown 中的相对图片路径，指向正确的资源位置

例如 `part1-image-processing/数字图像的获取和表示/practice.ipynb` 的 ID 为 `数字图像的获取和表示-practice`。

## 图片资源处理

Markdown 单元格中的相对图片路径会被自动重写：
- 原始路径 `![lena](lena.jpeg)`
- 重写为 `<img src="./notebooks/part1-image-processing/数字图像的获取和表示/lena.jpeg">`
- 绝对 URL 和 data: URI 不受影响

部署时需要将 notebooks 目录复制到构建输出目录（docs/notebooks/），使图片可访问。

## 核心组件

| 组件 | 功能 |
|------|------|
| `Sidebar.jsx` | 左侧边栏导航，按部分组织教程 |
| `NotebookViewer.jsx` | Notebook 渲染器，显示内容和右侧大纲 |
| `Welcome.jsx` | 首页欢迎页，展示课程概览 |
| `NotesPanel.jsx` | 笔记和书签管理面板 |
| `SettingsPanel.jsx` | 设置面板（主题、字号） |
| `ImageLightbox.jsx` | 图片灯箱查看器 |

## 核心数据模块

**data/notebooks.js**:
- 使用 `import.meta.glob('../../../notebooks/**/*.ipynb', { query: '?raw', import: 'default' })` 动态加载
- 嵌套路径正则匹配：`rootDir/([^/]+)/(.+?)\.ipynb$`
- 计算 imageBase 路径用于图片 URL 重写
- 实现 Markdown 渲染（标题、列表、表格、引用、代码块）
- 实现 Python 代码高亮
- 实现 Notebook 单元格渲染（markdown cell, code cell, output）
- 支持 KaTeX 数学公式渲染
- 实现内存缓存和预取机制

**data/sidebar.js**:
- 定义学习路径（PATH_STEPS）
- 定义精选笔记本（RUNNABLE_NOTEBOOKS）
- lessonId 必须与 notebooks.js 生成的 ID 匹配

## Vite 配置

**vite.config.js** 关键配置：
- 自定义虚拟模块插件 `virtual:notebook-catalog`
- 递归扫描 notebooks 目录，生成 catalog
- 嵌套路径正则：`rel.match(/^([^/]+)\/(.+)\.ipynb$/)`
- ID 生成：`idPath.replace(/\//g, '-')`
- 从 Notebook JSON 提取第一个 `#` 标题作为显示标题
- 构建输出到 `../docs` 目录
- `base: './'` 确保相对路径

## 主题自定义

修改 `web/src/styles/index.css` 中的 CSS 变量。橙色主题示例：

```css
:root {
  --accent: #ea580c;                    /* 主色调 */
  --accent-soft: rgba(234, 88, 12, 0.09);
  --brand-gradient-from: rgba(234, 88, 12, 0.11);
  --brand-gradient-to: rgba(251, 146, 60, 0.12);
  --brand-accent: #ea580c;
  --code-inline-bg: #fff7ed;
  --code-inline-text: #c2410c;
  --blockquote-bg: #fff7ed;
  --blockquote-border: rgba(234, 88, 12, 0.1);
}

[data-theme="dark"] {
  --accent: #fb923c;
  --accent-soft: rgba(251, 146, 60, 0.12);
  --brand-gradient-from: rgba(251, 146, 60, 0.11);
  --brand-gradient-to: rgba(255, 180, 0, 0.12);
  --brand-accent: #fb923c;
}
```

## GitHub Actions 部署

**.github/workflows/deploy.yml** 关键步骤：

1. 递归复制中文目录到 notebooks 结构：
```bash
cp -r ../图像处理基础/* ../notebooks/part1-image-processing/
cp -r ../最优化算法与立体视觉重建/* ../notebooks/part2-optimization-3d/
```

2. 安装依赖（使用 npm install，非 npm ci）：
```bash
cd web && npm install
```

3. 构建：
```bash
npm run build
```

4. 复制 notebook 资源（图片等）到构建输出：
```bash
mkdir -p ../docs/notebooks
cp -r ../notebooks/* ../docs/notebooks/
```

5. 上传并部署到 GitHub Pages

注意：GitHub Pages 需配置为使用 GitHub Actions 作为部署源。

## Notebook 组织规范

教程按部分组织，每个部分一个顶层文件夹：
- `part1-image-processing/` - 图像处理基础
- `part2-optimization-3d/` - 最优化与立体视觉

每个部分内可以有子目录，每个子目录包含：
- `practice.ipynb` - 主练习 Notebook
- `practice_extra.ipynb` - 拓展练习（可选）
- 图片资源文件（.jpg, .png, .jpeg）

确保每个 Notebook 的第一个 markdown cell 是一级标题（# 标题），用于自动提取显示名称。

## 最佳实践

1. **Notebook 结构**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的注释
3. **自包含**: 每篇 Notebook 应该可以独立运行，不依赖前面的状态
4. **图片资源**: 图片放在 Notebook 同目录下，使用相对路径引用
5. **固定种子**: 涉及随机实验时使用固定种子，确保可复现
6. **npm install**: 在 GitHub Actions 中使用 npm install 而非 npm ci，避免 package-lock.json 依赖问题

## 参考模板

本 skill 基于 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板。
