# Learning Tutorial Site

基于 Jupyter Notebook 的交互式学习教程网站生成器。

> 参考 [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) 项目模板构建的 TRAE Skill

## 功能特性

- **直接渲染 .ipynb 文件** - 无需后端，前端直接解析 Notebook
- **中英双语切换** - 支持多语言教程
- **侧边栏目录导航** - 按学习路径组织内容
- **右侧大纲导航** - 快速跳转到章节
- **代码语法高亮** - Python 代码智能高亮
- **Markdown 完整渲染** - 标题、列表、表格、引用、代码块
- **数学公式支持** - KaTeX 渲染 LaTeX 公式
- **浅色/深色主题** - 三种主题模式切换
- **字号调整** - 小/中/大三档字号
- **笔记和书签** - 本地存储学习笔记
- **文字选中工具栏** - 复制、笔记、高亮、分享
- **代码折叠展开** - 长代码默认折叠
- **输出折叠展开** - 长输出默认折叠
- **图片灯箱** - 点击放大查看
- **新手引导** - 交互式教程引导
- **更新日志** - Git log 自动生成
- **URL hash 路由** - 支持分享链接
- **响应式设计** - 移动端完美适配
- **预取缓存** - 智能预取提升切换速度

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
# 在你的工作区根目录下创建 .trae/skills 目录
mkdir -p .trae/skills

# 克隆本仓库
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
tutorial-site/                    # 生成的教程网站项目
├── notebooks/                    # 中文教程笔记本（.ipynb 文件）
│   ├── part1-foundation/         # 基础篇
│   ├── part2-training/           # 训练篇
│   ├── part3-inference/          # 推理篇
│   ├── part4-frontiers/          # 前沿篇
│   └── appendix-advanced/        # 附录
├── notebooks-en/                 # 英文教程笔记本（可选）
├── web/                          # React/Vite 前端网站
│   ├── src/
│   │   ├── components/           # React 组件
│   │   │   ├── Sidebar.jsx
│   │   │   ├── NotebookViewer.jsx
│   │   │   ├── Welcome.jsx
│   │   │   ├── NotesPanel.jsx
│   │   │   ├── SettingsPanel.jsx
│   │   │   ├── GuidedTour.jsx
│   │   │   ├── ChangelogModal.jsx
│   │   │   └── ImageLightbox.jsx
│   │   ├── context/              # Context 状态管理
│   │   ├── data/                 # 数据配置
│   │   │   ├── notebooks.js      # Notebook 加载与渲染
│   │   │   └── sidebar.js        # 侧边栏数据
│   │   ├── hooks/                # 自定义 Hooks
│   │   │   ├── useSettings.js
│   │   │   ├── useTheme.js
│   │   │   └── useNotesAndBookmarks.js
│   │   ├── styles/               # 全局样式
│   │   ├── utils/                # 工具函数
│   │   ├── App.jsx
│   │   ├── config.js
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
├── scripts/                      # 维护脚本
├── requirements.txt              # Python 依赖
├── package.json                  # 根 package.json
└── README.md
```

## 快速开始

### 1. 初始化项目

在 TRAE 中说「创建一个学习教程网站」，skill 会自动帮你生成完整的项目结构。

### 2. 添加教程内容

将你的 `.ipynb` 文件放入 `notebooks/` 对应的部分目录中，文件命名格式：

```
{序号}-{主题}.ipynb
```

例如：`01-introduction.ipynb`

### 3. 配置侧边栏

编辑 `web/src/data/sidebar.js`，配置你的学习路径和精选笔记本。

### 4. 启动开发服务器

```bash
cd web
npm install
npm run dev
```

### 5. 构建生产版本

```bash
cd web
npm run build
# 输出到 ../docs 目录
```

## 配置自定义教程

### 修改网站标题

修改 `web/index.html` 中的 `<title>` 标签。

### 自定义主题色

修改 `web/src/styles/index.css` 中的 CSS 变量：

```css
:root {
  --accent: #1d6bf3;           /* 主色调 */
  --brand-gradient-from: ...;  /* 渐变起始色 */
  --brand-gradient-to: ...;    /* 渐变结束色 */
}
```

### 添加新的学习路径

编辑 `web/src/data/sidebar.js` 中的 `PATH_STEPS` 数组。

## 最佳实践

1. **Notebook 结构**: 每篇 Notebook 遵循「直觉 → 手算 → 实现 → 实验」的学习路径
2. **代码可读性**: 保持代码单元格短小，添加充分的注释
3. **双语一致性**: 中英文版本的 Notebook 编号和结构保持对应
4. **自包含**: 每篇 Notebook 应该可以独立运行，不依赖前面的状态
5. **检查清单**: 每篇 Notebook 末尾添加总结检查清单
6. **固定种子**: 涉及随机实验时使用固定种子，确保可复现

## 参考项目

- [modern-llm-notebook](https://github.com/walkinglabs/modern-llm-notebook) - 本 skill 的参考模板

## 许可证

MIT License
