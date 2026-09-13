# Learning Tutorial Site

基于 Jupyter Notebook 的交互式学习教程网站生成器。

> 参考 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板构建的 TRAE Skill

## 功能特性

- **直接渲染 .ipynb 文件** - 无需后端，前端直接解析 Notebook
- **嵌套目录支持** - Notebook 可放在任意深度的子目录中
- **图片路径自动重写** - Markdown 中的相对图片路径自动指向正确位置
- **侧边栏目录导航** - 按学习路径组织内容
- **右侧大纲导航** - 快速跳转到章节
- **代码语法高亮** - Python 代码智能高亮
- **Markdown 完整渲染** - 标题、列表、表格、引用、代码块
- **数学公式支持** - KaTeX 渲染 LaTeX 公式
- **浅色/深色主题** - 主题模式切换，支持自定义颜色
- **字号调整** - 小/中/大三档字号
- **笔记和书签** - 本地存储学习笔记
- **代码折叠展开** - 长代码默认折叠
- **输出折叠展开** - 长输出默认折叠
- **图片灯箱** - 点击放大查看
- **URL hash 路由** - 支持分享链接
- **响应式设计** - 移动端完美适配
- **预取缓存** - 智能预取提升切换速度
- **GitHub Actions 自动部署** - 推送即部署到 GitHub Pages

## 技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| React | 19 | 前端框架 |
| Vite | 6 | 构建工具 |
| Tailwind CSS | 4 | 样式框架 |
| KaTeX | 0.17 | 数学公式渲染 |
| Lucide React | 0.546 | 图标库 |

## Skill 安装

### 方式一：克隆到工作区

```bash
mkdir -p .trae/skills
cd .trae/skills
git clone https://github.com/ZOUMDA28/learning-tutorial-site.git
```

### 方式二：手动复制

将本仓库中的 `.trae/skills/learning-tutorial-site/` 目录复制到你的工作区 `.trae/skills/` 目录下。

## 使用方法

在 TRAE 对话中，当你说以下内容时，该 skill 会被自动调用：

- 「创建一个学习教程网站」
- 「生成 Notebook 教程网站」
- 「做一个在线课程网站」
- 「把 ipynb 转成网页」

## 项目结构

```
tutorial-site/
├── notebooks/                    # 教程笔记本
│   ├── part1-image-processing/   # 第一部分
│   │   ├── 数字图像的获取和表示/  # 子目录（支持嵌套）
│   │   │   ├── practice.ipynb
│   │   │   └── lena.jpeg         # 图片资源
│   │   └── 几何变换/
│   │       └── practice.ipynb
│   └── part2-optimization-3d/    # 第二部分
├── web/                          # React/Vite 前端网站
│   ├── src/
│   │   ├── components/           # React 组件
│   │   ├── context/              # Context 状态管理
│   │   ├── data/                 # 数据配置
│   │   │   ├── notebooks.js      # Notebook 加载与渲染
│   │   │   └── sidebar.js        # 侧边栏数据
│   │   ├── hooks/                # 自定义 Hooks
│   │   ├── styles/               # 全局样式
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
├── .github/workflows/           # GitHub Actions
│   └── deploy.yml               # 自动部署工作流
└── README.md
```

## 嵌套目录支持

Notebook 可以放在任意深度的子目录中。系统会自动：
- 递归扫描所有 .ipynb 文件
- 使用 `{子目录路径}-{文件名}` 作为唯一 ID
- 从 Notebook 的第一个 markdown 一级标题提取标题
- 重写 Markdown 中的相对图片路径

## 图片资源处理

Markdown 中的相对图片路径会被自动重写为正确的绝对路径。部署时需将 notebooks 目录复制到构建输出目录。

## 快速开始

### 1. 初始化项目

在 TRAE 中说「创建一个学习教程网站」，skill 会自动帮你生成完整的项目结构。

### 2. 添加教程内容

将你的 `.ipynb` 文件放入 `notebooks/` 对应的部分目录中。

### 3. 启动开发服务器

```bash
cd web
npm install
npm run dev
```

### 4. 部署到 GitHub Pages

配置 `.github/workflows/deploy.yml`，推送到 main 分支即可自动部署。

## 自定义主题色

修改 `web/src/styles/index.css` 中的 CSS 变量：

```css
:root {
  --accent: #ea580c;           /* 主色调（橙色示例） */
  --brand-gradient-from: ...;  /* 渐变起始色 */
  --brand-gradient-to: ...;    /* 渐变结束色 */
}
```

## 最佳实践

1. **Notebook 结构**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的注释
3. **自包含**: 每篇 Notebook 应该可以独立运行
4. **图片资源**: 图片放在 Notebook 同目录下，使用相对路径引用
5. **固定种子**: 涉及随机实验时使用固定种子
6. **npm install**: 在 GitHub Actions 中使用 npm install 而非 npm ci

## 参考项目

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 本 skill 的参考模板

## 许可证

MIT License
