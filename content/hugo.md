+++
title = 'Hugo'
date = 2023-09-20T20:07:38+08:00
draft = false
+++

使用[hugo](https://gohugo.io)搭建博客并搭配[xmin](https://github.com/yihui/hugo-xmin)主题。

## hugo安装与初始化

```shell
brew install hugo            # 安装
hugo new site /path/to/site  # 初始化
cd /path/to/site             # 切换目录

git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
hugo new content posts/my-first-post.md
hugo server --buildDrafts
hugo server -D
```

## 创建页面

hugo new post/first.md 新文章
hugo new about.md 新页面