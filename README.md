# 个人博客网站

这是一个基于Nuxt 3和TypeScript构建的个人博客网站，包含照片墙、个人简介、兴趣爱好、音乐分享和电影分享等功能。

## 功能特点

- 响应式设计，适配各种设备
- 暗黑模式支持
- 照片墙展示，支持分类筛选
- 音乐分享，支持模拟播放
- 电影分享，支持收藏和标记已看
- 个人简介和兴趣爱好展示

## 技术栈

- [Nuxt 3](https://nuxt.com/) - Vue.js框架
- [TypeScript](https://www.typescriptlang.org/) - 类型安全的JavaScript
- [Tailwind CSS](https://tailwindcss.com/) - 实用优先的CSS框架
- [Iconify](https://iconify.design/) - 图标库
- [Pinia](https://pinia.vuejs.org/) - Vue.js状态管理
- [VueUse](https://vueuse.org/) - Vue组合式API工具集

## 本地开发

### 前提条件

- [Node.js](https://nodejs.org/) (v16+)
- [Bun](https://bun.sh/) 包管理器

### 安装依赖

```bash
bun install
```

### 启动开发服务器

```bash
bun run dev
```

访问 http://localhost:3000 查看网站。

## 构建部署

### 构建生产版本

```bash
bun run build
```

### 本地预览生产版本

```bash
bun run preview
```

### 部署到Cloudflare Pages

1. 在Cloudflare Pages中创建一个新项目
2. 连接你的GitHub仓库
3. 设置构建命令为 `bun run build`
4. 设置构建输出目录为 `.output/public`
5. 点击部署

## 项目结构

```
linyituo-blog/
├── assets/            # 静态资源
│   ├── css/           # CSS文件
│   └── images/        # 图片文件
├── components/        # Vue组件
├── composables/       # 组合式函数
├── content/           # 内容文件
│   ├── blog/          # 博客文章
│   ├── movies/        # 电影数据
│   ├── music/         # 音乐数据
│   └── photos/        # 照片数据
├── layouts/           # 布局组件
├── pages/             # 页面组件
├── public/            # 公共文件
├── app.vue            # 应用入口
├── nuxt.config.ts     # Nuxt配置
└── tsconfig.json      # TypeScript配置
```

## 自定义

- 修改 `nuxt.config.ts` 文件中的网站标题和描述
- 在 `pages/` 目录中编辑页面内容
- 在 `layouts/default.vue` 中修改网站布局
- 在 `content/` 目录中添加或修改内容数据

## 许可证

MIT
