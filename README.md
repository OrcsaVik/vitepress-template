这是一份为你优化后的 `README.md`。我引入了 Shields.io 风格的**技术栈勋章**，并重点重构了关于**如何编写与组织内容**的部分，帮助你更规范地维护博客。

---

# OrcsaVik.github.io 个人博客

<p align="center">
<img src="./docs/public/logo.png" width="100" />
</p>

<p align="center">
<strong>基于 VitePress 构建的现代化、高性能个人技术知识库</strong>
</p>

<p align="center">
<img src="[https://img.shields.io/badge/VitePress-1.6.4-646cff?style=flat-square&logo=vite&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/VitePress-1.6.4-646cff%3Fstyle%3Dflat-square%26logo%3Dvite%26logoColor%3Dwhite)" />
<img src="[https://img.shields.io/badge/Vue-3.5.26-42b883?style=flat-square&logo=vuedotjs&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/Vue-3.5.26-42b883%3Fstyle%3Dflat-square%26logo%3Dvuedotjs%26logoColor%3Dwhite)" />
<img src="[https://img.shields.io/badge/TypeScript-Latest-3178c6?style=flat-square&logo=typescript&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/TypeScript-Latest-3178c6%3Fstyle%3Dflat-square%26logo%3Dtypescript%26logoColor%3Dwhite)" />
<img src="[https://img.shields.io/badge/Search-Algolia-003dff?style=flat-square&logo=algolia&logoColor=white](https://www.google.com/search?q=https://img.shields.io/badge/Search-Algolia-003dff%3Fstyle%3Dflat-square%26logo%3Dalgolia%26logoColor%3Dwhite)" />
<img src="[https://img.shields.io/badge/License-MIT-yellow?style=flat-square](https://www.google.com/search?q=https://img.shields.io/badge/License-MIT-yellow%3Fstyle%3Dflat-square)" />
</p>

---

## 📖 项目介绍

本项目是我的个人技术沉淀空间。不仅包含技术教程，还涵盖了系统设计、开源项目维护及读书感悟。通过 VitePress 的极速构建能力，实现“**所写即所得**”的写作体验。

---

## ✍️ 内容创作指南（重构重点）

为了保持博客的长期可维护性，建议遵循以下流程添加新文章：

### 1. 文章分类逻辑

所有文章应存放于 `docs/articles/` 及其对应的技术子目录中。

* **Java/C/Python**: 语言基础与进阶
* **Front**: 前端工程化与框架
* **LLM**: 大模型应用与 Prompt 工程
* **System-Design**: 架构思维与实战

### 2. 标准 Frontmatter 配置

每篇 `.md` 文件的顶部必须包含以下元数据，以便插件识别和 SEO 优化：

```markdown
---
title: 文章标题
editLink: true
outline: [2, 3] # 显示2-3级标题在右侧大纲
date: 2024-03-20
---

```

### 3. 使用高级组件

博客集成了图标管理和交互插件，可在正文中使用：

* **代码组图标**：使用 `::: code-group` 配合插件展示不同语言图标。
* **提示区块**：合理使用 `::: tip`, `::: info`, `::: danger` 引导阅读。



## 🏗️ 如何添加新专栏（导航机制）

如果你想增加一个全新的领域（例如 Rust 专栏），请遵循以下三个简单步骤：

---

### 第一步：创建文件夹

在 `docs/` 目录下创建你的新专栏文件夹，并放入 `.md` 文件。

```
docs/
└── rust/
    ├── index.md        # 专栏首页
    └── basic-syntax.md # 第一篇专栏文章
```

> 💡 提示：`index.md` 是该目录的默认入口页面，访问 `/rust/` 时会自动加载它。

---

### 第二步：配置导航栏 (Nav)

编辑 `docs/.vitepress/config.mts`（或对应的配置文件），将新专栏加入顶部导航：

```ts
// .vitepress/config.mts
export default {
  themeConfig: {
    nav: [
      { text: 'Rust 学习', link: '/rust/index' }, // 指向新专栏首页
      // ... 其他导航项
    ]
  }
}
```

> ✅ 注意：`link` 路径应以 `/` 开头，指向你专栏的首页或具体文章。

---

### 第三步：配置自动侧边栏 (Sidebar)

得益于 `vitepress-sidebar` 插件（或 VitePress 内置的自动侧边栏功能），你只需在 `docs/.vitepress/sidebar/` 下新建一个 `rust.ts` 文件并导出配置，或者直接在主配置中映射该文件夹：

#### 方式一：使用独立侧边栏配置文件（推荐）

```ts
// docs/.vitepress/sidebar/rust.ts
export default [
  {
    text: 'Rust 专栏',
    collapsed: false,
    items: [
      { text: '语法基础', link: '/rust/basic-syntax' },
      // 可继续添加更多文章
    ]
  }
]
```




然后在主配置中引入：

```ts
// .vitepress/config.mts
import rustSidebar from './sidebar/rust'

export default {
  themeConfig: {
    sidebar: {
      '/rust/': rustSidebar, // 当访问 /rust/ 路径时，使用此侧边栏
      // 其他路径可配置不同侧边栏
    }
  }
}
```


且在本项目集成一个ts 为每个专栏自动设置侧边栏

docs\.vitepress\sidebar\options.ts

```ts
// 2. Define distinct sections
// We export an ARRAY of options. The plugin will merge them.
export const sidebarOptions: GenerateSidebarOption[] = [
  // === Articles: Java ===
  {
    ...commonOptions,
    scanStartPath: 'articles/java', // Folder to scan
    resolvePath: '/articles/java/', // Only show this sidebar on these URLs
    rootGroupText: 'Java 专栏',    // Optional: Label for the top group
  },
  // === Articles: Python ===
  {
    ...commonOptions,
    scanStartPath: 'articles/python',
    resolvePath: '/articles/python/',
    rootGroupText: 'Python 专栏',
  },
  // === Articles: LLM ===
  {
    ...commonOptions,
    scanStartPath: 'articles/llm',
    resolvePath: '/articles/llm/',
    rootGroupText: 'LLM 专栏',
  },
  // === System Design ===
  {
    ...commonOptions,
    scanStartPath: 'system-design',
    resolvePath: '/system-design/',
    rootGroupText: '系统设计',
  },
  // === Open Source ===
  {
    ...commonOptions,
    scanStartPath: 'open-source',
    resolvePath: '/open-source/',
    rootGroupText: '开源项目',
  },
```


只需要重新追加且指定渲染路径

  // === Open Source ===
  {
    ...commonOptions,
    scanStartPath: 'rust',
    resolvePath: '/rust/',
    rootGroupText: 'Rust 专栏',
  },



#### 方式二：手动配置（适合小型项目）

直接在主配置中定义：

```ts
// .vitepress/config.mts
export default {
  themeConfig: {
    sidebar: {
      '/rust/': [
        {
          text: 'Rust 专栏',
          collapsed: false,
          items: [
            { text: '语法基础', link: '/rust/basic-syntax' },
          ]
        }
      ]
    }
  }
}
```

> 🎯 优势：VitePress 支持“按路径匹配”侧边栏，这样不同专栏可以拥有独立的导航结构。

---

## 📁 推荐目录结构

```
docs/
├── .vitepress/           # VitePress 核心配置
│   ├── config.mts        # 主配置文件
│   ├── sidebar/          # 侧边栏配置（按专栏拆分）
│   │   ├── rust.ts       # Rust 专栏侧边栏
│   │   └── java.ts       # Java 专栏侧边栏
│   └── theme/            # 自定义主题与 CSS
├── articles/             # 📚 技术文章（Java, 前端, LLM 等）
├── system-design/        # 🏗️ 系统设计专题
├── open-source/          # 🌟 开源项目记录
└── public/               # 静态资源（图片、Logo、favicon 等）
```

---

## ✅ 总结：添加新专栏三步走

1. **建文件夹** → 在 `docs/` 下创建新专栏目录，放入 `.md` 文件
2. **加导航** → 在 `config.mts` 的 `nav` 中添加入口链接
3. **配侧边栏** → 在 `sidebar/` 下创建对应配置文件，或在主配置中映射路径

---

## 🧩 小贴士

- 使用 `collapsed: false` 让侧边栏默认展开，方便阅读
- 可通过 `items` 数组自由组织文章顺序和层级
- 如果专栏文章较多，建议按模块再细分子目录（如 `/rust/basics/`, `/rust/advanced/`）


---

## 🛠️ 技术实现

### 核心插件列表

| 插件 | 用途 | 勋章 |
| --- | --- | --- |
| **vitepress-sidebar** | 根据文件夹结构**全自动**生成侧边栏 | 📋 |
| **plugin-group-icons** | 在代码块头部自动识别并显示语言图标 | 🎨 |
| **Algolia Search** | 提供毫秒级的全文搜索体验 | 🔍 |
| **canvas-confetti** | 为交互动作添加炫酷的纸屑特效 | 🥳 |

---

## 🚀 快速开始

### 1. 环境准备

确保本地安装了 Node.js (建议 v18+)

```bash
# 克隆仓库
git clone https://github.com/OrcsaVik/OrcsaVik.github.io.git

# 安装依赖
npm install

```

### 2. 开发与构建

```bash
npm run docs:dev      # 启动本地开发预览 (http://localhost:5173)
npm run docs:build    # 构建静态文件到 .vitepress/dist
npm run docs:preview  # 预览构建后的生产版本

```

---

## 📫 联系方式

* **Github**: [OrcsaVik](https://www.google.com/search?q=https://github.com/OrcsaVik)
* **Website**: [OrcsaVik.github.io](https://www.google.com/search?q=https://OrcsaVik.github.io)

---
