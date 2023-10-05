+++
title = 'Hugo主题开发'
date = 2023-09-28T15:02:52+08:00
draft = false
categories = ["博客"]
tags = ["博客","hugo"]
toc = true
+++

本文记录基于 [xmin](https://github.com/yihui/hugo-xmin) 的主题开发，对大体框架内容进行概述，不做具体细节描述（对应细节位置会放置文档链接）。

xmin主题代码量很少，而且结构清晰，适合用来自定义主题。对xmin感兴趣的可以查看[demo](https://xmin.yihui.org)。

## 主题框架

```text
hugo-xmin/
├── LICENSE.md
├── README.md
├── archetypes
│   └── default.md  ## 生成文章时的默认模板
├── layouts ## 布局
│   ├── 404.html ## 404页面
│   ├── _default ## 默认布局
│   │   ├── list.html ## 呈现页面列表的模板，例如博客文章列表或类别或标记中的页面列表
│   │   ├── single.html ## 单文章模板，即content/posts/*.md 使用的模板
│   │   └── terms.html ## 是分类术语主页的模板
│   └── partials ## 部分复用模板
│       ├── foot_custom.html
│       ├── footer.html
│       ├── head_custom.html
│       └── header.html
├── static
│   └── css ## css文件
│       ├── fonts.css
│       └── style.css
```
