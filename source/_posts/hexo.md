---
title: "Hexo安装过程" 
author: Gweek
draft: false
date: 2024-08-30
tags:
    - hexo
thumbnail: "https://img.myla.eu.org/api/file/790de744b616f5b749b2e.png"
---
### Hexo 是什么

Hexo 是一个基于 Node.js 的静态博客框架，广泛用于搭建快速、简洁且高效的博客或静态网站。它的主要特点是生成速度快、易于配置和扩展，支持丰富的主题和插件，适合于技术人员和博主快速搭建个人博客或文档站点。

### Hexo 的工作原理

Hexo 采用 Markdown 或其他标记语言编写内容，然后通过模板引擎生成静态 HTML 文件，这些文件可以直接托管在任何静态网站托管平台上（如 GitHub Pages、Vercel、Netlify 等）。

### Hexo 项目文件结构

```
.
├── _config.yml            # Hexo 主配置文件，控制网站的基本设置
├── package.json           # Node.js 项目的配置文件，包含 Hexo 的依赖项
├── scaffolds/             # 模板文件夹，存放文章、页面等内容的模板
│   ├── draft.md           # 草稿模板
│   ├── page.md            # 页面模板
│   └── post.md            # 文章模板
├── source/                # 资源文件夹，存放用户的文章、页面和其他资源
│   ├── _drafts/           # 草稿文件夹，未发布的文章存放在此处
│   ├── _posts/            # 已发布的文章，所有 Markdown 格式的文章文件都放在这里
│   └── ...                # 其他资源文件（如图片、文档等），可以直接存放于此
├── themes/                # 主题文件夹，存放 Hexo 使用的主题
│   └── your-theme/        # 当前使用的主题目录
│       ├── _config.yml    # 主题的单独配置文件
│       ├── layout/        # 页面布局文件夹
│       ├── source/        # 主题相关资源，如图片、CSS、JS
│       └── ...            # 其他主题相关文件
├── public/                # 生成的静态文件，`hexo generate` 后输出的 HTML 文件
└── node_modules/          # Node.js 依赖包文件夹，存放 Hexo 和插件的依赖
```

### 主要文件及文件夹详解

1. **`_config.yml`**：Hexo 的主配置文件，定义了网站的基本设置（如站点名称、描述、语言、URL 配置、部署方式等）。这是整个网站的核心配置文件。
2. **`package.json`**：定义了 Hexo 项目的依赖和脚本，可以在此添加或更新 Hexo 相关的插件、主题等内容。
3. **`scaffolds/`**：用于存放文章、页面等内容的模板文件，当创建新文章或页面时，会按照模板内容进行初始化。用户可以根据需要自定义这些模板。
4. **`source/`**：是用户存放内容的主要目录：
    - **`_posts/`**：放置已发布的文章，文章文件通常以 Markdown 格式保存。
    - **`_drafts/`**：存放未发布的草稿文章，通过 `hexo publish` 命令可以将草稿发布到 `_posts` 目录。
    - 其他文件或文件夹：可以存放资源文件（如图片、PDF 等），会直接映射到网站的根目录。
5. **`themes/`**：用于存放 Hexo 的主题，用户可以选择或自定义主题来改变网站的外观。每个主题都有其独立的配置文件 `_config.yml`，用于设置主题相关的参数。
6. **`public/`**：生成的静态文件存放在这个目录，通过 `hexo generate` 命令生成。此目录中的文件可以直接上传到 Web 服务器或托管服务进行发布。
7. **`node_modules/`**：存放 Hexo 项目依赖的 Node.js 模块和插件，通常不需要手动修改。

### Hexo 文件结构的意义

- **模块化管理**：不同功能、内容和配置分离，方便管理和维护。
- **扩展性强**：通过 `themes` 和 `scaffolds` 等文件夹，用户可以方便地进行自定义。
- **生成速度快**：Hexo 只需读取 `source` 和 `scaffolds` 中的内容，通过主题生成静态页面，所有生成内容都保存在 `public` 文件夹中，速度快且易于部署。

### Hexo 的搭建步骤

**Step 1: 安装 Node.js**

Hexo 依赖于 Node.js，因此需要先安装 Node.js 和 npm（Node.js 的包管理工具）。

1. **下载 Node.js：**
    - 访问 [Node.js 官方网站](https://nodejs.org/)。
    - 推荐下载 LTS（长期支持）版本，以保证稳定性。
2. **安装 Node.js：**
    - Windows 用户：运行下载的 `.msi` 安装程序，按照提示完成安装。
    - macOS 用户：运行下载的 `.pkg` 文件进行安装。
    - Linux 用户：可以通过包管理器安装，如：
        
        ```bash
        # Debian/Ubuntu
        sudo apt update
        sudo apt install -y nodejs npm
        
        # 或者通过 NodeSource 安装最新版本
        curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
        sudo apt-get install -y nodejs
        ```
        
3. **验证 Node.js 和 npm 是否安装成功：**
    
    ```bash
    node -v
    npm -v
    ```
    
    运行以上命令会输出 Node.js 和 npm 的版本号。
    

**Step 2: 安装 Git**

Git 是 Hexo 部署和版本控制所需的工具。

1. **下载 Git：**
    - 访问 [Git 官方网站](https://git-scm.com/) 下载适用于系统的安装包。
2. **安装 Git：**
    - Windows 用户：运行下载的 `.exe` 文件，并按照提示进行安装。
    - macOS 用户：可以通过下载 `.dmg` 文件安装，或使用 Homebrew 安装：
        
        ```bash
        brew install git
        ```
        
    - Linux 用户：可以通过包管理器安装，如：
        
        ```bash
        # Debian/Ubuntu
        sudo apt update
        sudo apt install -y git
        ```
        
3. **验证 Git 是否安装成功：**
    
    ```bash
    git --version
    ```
    
    运行该命令会显示 Git 的版本号。
    

**Step 3: 安装 Hexo**

1. **安装 Hexo CLI：**
使用 npm 安装 Hexo 的命令行工具（CLI）。
    
    ```bash
    npm install -g hexo-cli
    ```
    
    - `g` 选项表示全局安装，使得 Hexo 命令在系统中全局可用。
2. **创建 Hexo 项目：**
在你想创建博客的目录中运行以下命令：
    
    ```bash
    hexo init my-blog
    cd my-blog
    npm install
    ```
    
    - `hexo init my-blog`：在当前目录下创建一个名为 `my-blog` 的新 Hexo 项目。
    - `cd my-blog`：进入项目文件夹。
    - `npm install`：安装项目依赖。
3. **配置 Hexo：**
打开项目中的 `_config.yml` 文件，修改站点的基本配置（如站点名称、描述、作者信息等）。
4. **运行 Hexo：**
    - 生成静态文件：
        
        ```bash
        hexo generate
        ```
        
    - 本地启动服务器预览网站：
        
        ```bash
        hexo server
        ```
        
    
    访问 `http://localhost:4000`，即可预览你的网站。
    
5. **部署 Hexo 网站：**
    - 配置 `_config.yml` 中的 `deploy` 选项，设置部署方式（例如 GitHub Pages）。
    - 安装部署插件（如 Git 部署插件）：
        
        ```bash
        npm install hexo-deployer-git --save
        ```
        
    - 运行部署命令：
        
        ```bash
        hexo deploy
        ```
