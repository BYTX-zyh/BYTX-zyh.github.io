+++
title = 'Hugo使用'
date = 2023-09-25T15:09:23+08:00
draft = false
categories = ["博客"]
tags = ["博客"]
toc = true
+++

# test

## mathjax

[mathjax](https://www.mathjax.org)支持可以查看[此处](https://www.mathjax.org/#gettingstarted),将如下代码添加到主题的`footer_custom.html`中：

```javascript
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
```

## code highlight

hugo 默认使用 [chroma](https://github.com/alecthomas/chroma) 进行代码高亮，可以在[playground](https://swapoff.org/chroma/playground/)进行主题预览。



## 添加 html 代码

如果需要在 Markdown 源文件中添加 html 代码，默认情况下无法显示对应的内容，需要在配置文件中添加如下字段：

```toml
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```