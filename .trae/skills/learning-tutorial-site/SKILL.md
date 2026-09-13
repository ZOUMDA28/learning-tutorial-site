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
├── notebooks/           # 中文教程笔记本（.ipynb 文件）
│   ├── part1-foundation/
│   ├── part2-training/
│   ├── part3-inference/
│   └── part4-frontiers/
├── notebooks-en/        # 英文教程笔记本（可选）
├── web/                 # React/Vite 前端网站
│   ├── src/
│   │   ├── components/  # React 组件
│   │   ├── context/     # Context 状态管理
│   │   ├── data/        # 数据配置
│   │   ├── hooks/       # 自定义 Hooks
│   │   ├── styles/      # 全局样式
│   │   ├── utils/       # 工具函数
│   │   ├── App.jsx
│   │   ├── config.js
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
├── scripts/             # 维护脚本
├── requirements.txt     # Python 依赖
├── package.json         # 根 package.json
└── README.md
```

## 创建步骤

### 1. 初始化项目结构

创建以下目录结构：
- `notebooks/` - 存放中文 .ipynb 教程
- `notebooks-en/` - 存放英文 .ipynb 教程（如需要双语）
- `web/` - 前端网站代码

### 2. 配置前端项目

在 `web/` 目录下创建：

**package.json** - 包含 React、Vite、Tailwind CSS、KaTeX、lucide-react 等依赖

**vite.config.js** - 配置：
- React 插件
- Tailwind CSS 插件
- Notebook 目录虚拟模块插件（自动扫描 .ipynb 文件生成目录）
- Changelog 插件（从 git log 生成更新日志）
- 构建输出到 `../docs` 目录

**index.html** - 入口 HTML，配置字体和 MathJax

### 3. 核心组件

必须实现的组件：

| 组件 | 功能 |
|------|------|
| `Sidebar.jsx` | 左侧边栏导航，按部分组织教程 |
| `NotebookViewer.jsx` | Notebook 渲染器，显示内容和右侧大纲 |
| `Welcome.jsx` | 首页欢迎页，展示课程概览 |
| `NotesPanel.jsx` | 笔记和书签管理面板 |
| `SettingsPanel.jsx` | 设置面板（主题、字号） |
| `GuidedTour.jsx` | 新手引导教程 |
| `ChangelogModal.jsx` | 更新日志弹窗 |
| `ImageLightbox.jsx` | 图片灯箱查看器 |

### 4. 核心数据模块

**data/notebooks.js**:
- 使用 `import.meta.glob` 动态加载所有 .ipynb 文件
- 实现 Markdown 渲染（标题、列表、表格、引用、代码块）
- 实现 Python 代码高亮
- 实现 Notebook 单元格渲染（markdown cell, code cell, output）
- 支持 KaTeX 数学公式渲染
- 实现内存缓存和预取机制

**data/sidebar.js**:
- 定义学习路径（PATH_STEPS）
- 定义精选可运行笔记本（RUNNABLE_NOTEBOOKS）

### 5. 状态管理和 Hooks

| Hook | 功能 |
|------|------|
| `useSettings.js` | 设置持久化（localStorage） |
| `useTheme.js` | 主题切换（浅色/深色/系统） |
| `useNotesAndBookmarks.js` | 笔记和书签管理 |

### 6. 样式系统

使用 CSS 变量定义主题：
- 浅色主题变量（:root）
- 深色主题变量（[data-theme="dark"]）
- 包含背景、文字、边框、强调色等
- 支持字号调整（small/default/large）

### 7. Notebook 组织规范

教程按部分组织，每个部分一个文件夹：
- `part1-foundation/` - 基础篇
- `part2-training/` - 训练篇
- `part3-inference/` - 推理篇
- `part4-frontiers/` - 前沿篇
- `appendix-advanced/` - 附录

每个 Notebook 文件命名格式：`{序号}-{主题}.ipynb`，如 `01-tokenizer-basics.ipynb`

### 8. 功能特性清单

必选功能：
- [x] Jupyter Notebook 直接渲染
- [x] 中英文双语切换
- [x] 侧边栏目录导航
- [x] 右侧大纲导航（TOC）
- [x] 代码语法高亮（Python）
- [x] Markdown 完整渲染
- [x] KaTeX 数学公式
- [x] 浅色/深色主题
- [x] 字号调整
- [x] 笔记和书签功能
- [x] 文字选中工具栏（复制、笔记、高亮、分享）
- [x] 代码块折叠/展开
- [x] 输出折叠/展开
- [x] 图片灯箱
- [x] 新手引导教程
- [x] 更新日志
- [x] URL hash 路由
- [x] 响应式设计（移动端适配）
- [x] 预取缓存（提升切换速度）

### 9. 构建和部署

**开发模式**:
```bash
cd web
npm install
npm run dev
```

**构建生产版本**:
```bash
cd web
npm run build
# 输出到 ../docs 目录
```

**部署到 GitHub Pages**:
- 配置 GitHub Actions workflow
- 构建输出到 docs/ 目录
- 配置 Pages 从 docs 目录部署

## 配置自定义教程

### 修改侧边栏数据

编辑 `web/src/data/sidebar.js`:

```javascript
export const PATH_STEPS = [
  { num: "01", title: "基础", titleEn: "Foundation", desc: "...", descEn: "...", section: "foundation" },
  // 添加更多学习路径
]

export const RUNNABLE_NOTEBOOKS = [
  { id: "nb-1", lessonId: "01-intro", title: "入门介绍", titleEn: "Introduction", desc: "...", descEn: "...", section: "foundation", duration: 10 },
  // 添加更多精选笔记本
]
```

### 添加新的 Notebook

1. 在 `notebooks/partX-xxx/` 目录下创建 `.ipynb` 文件
2. 文件命名格式：`{序号}-{主题}.ipynb`
3. 确保第一个 markdown cell 是一级标题（# 标题）
4. 保存后，侧边栏会自动扫描并显示

### 配置网站标题

修改 `web/index.html` 中的 `<title>` 标签和 `web/src/config.js` 中的配置。

### 自定义主题色

修改 `web/src/styles/index.css` 中的 CSS 变量：
- `--accent` - 主色调
- `--brand-gradient-from` / `--brand-gradient-to` - 渐变颜色

## 最佳实践

1. **Notebook 结构**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的注释
3. **双语一致性**: 中英文版本的 Notebook 编号和结构保持对应
4. **自包含**: 每篇 Notebook 应该可以独立运行，不依赖前面的状态
5. **检查清单**: 每篇 Notebook 末尾添加总结检查清单
6. **固定种子**: 涉及随机实验时使用固定种子，确保可复现

## 参考模板

本 skill 基于 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板，该项目是一个从零构建现代 LLM 系统的交互式课程。
