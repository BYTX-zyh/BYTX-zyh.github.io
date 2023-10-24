+++
title = 'Hugo安装'
date = 2023-09-20T20:07:38+08:00
draft = false
categories = [] 
tags = ["hugo"]
toc = true
+++

使用[hugo](https://gohugo.io)搭建博客并搭配[xmin](https://github.com/yihui/hugo-xmin)主题，可以查看xmin的[demo](https://xmin.yihui.org)。

## 安装与初始化

```shell
brew install hugo            # 安装 hugo
hugo new site /path/to/site  # 初始化 hugo 博客文件夹
cd /path/to/site             # 切换目录

git init

## 添加主题
## 网页的布局将使用对应主题的archetypes、layouts和static配置
git submodule add https://github.com/yihui/hugo-xmin themes/hugo-xmin
echo "theme = 'hugo-xmin'" >> hugo.toml
hugo server


hugo new content posts/my-first-post.md
hugo server --buildDrafts
hugo server -D
```

## 创建页面

```shell
hugo new post/first.md #新文章
hugo new about.md      #新页面
```

## GitHub-pages 自动部署

### 初始化 GitHub仓库

创建GitHub仓库，并将本地仓库push到GitHub。

### 设置部署内容

在仓库主页进入 `Settings > Pages`，将 `Build and deployment > Source` 修改为 `Github Actions`。

本地仓库中创建一个新的文件 `.github/workflows/hugo.yaml`，并写入如下字段，
根据情况更改分支名称和 Hugo 版本。

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - master

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.118.2
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```