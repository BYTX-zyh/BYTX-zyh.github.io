+++
title = 'Hugo主题开发'
date = 2023-09-28T15:02:52+08:00
draft = false
categories = ["博客"]
tags = ["博客","hugo"]
toc = true
+++

本文记录基于 [xmin](https://github.com/yihui/hugo-xmin) 的主题开发，对大体框架内容进行概述，不做具体细节描述（对应细节位置会放置文档链接）。

xmin主题代码量少且结构清晰，适合用来自定义主题。对xmin感兴趣的可以查看[demo](https://xmin.yihui.org)。

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
│   └── js ## js文件
```

`archetypes/default.md` 为

## 模板语法

https://gohugo.io/templates/introduction/

### 基础语法

```html
<!--访问预定义变量-->
{{ .Title }}
{{ $address }}

<!--函数调用-->
{{ FUNCTION ARG1 ARG2 .. }}

<!--访问Markdown前言部分 Front matter variables-->
{{ .Params.bar }}

<!--使用括号分组-->
{{ if or (isset .Params "alt") (isset .Params "caption") }} Caption {{ end }}

<!--单个语句可以拆分多行-->
{{ if or
(isset .Params "alt")
(isset .Params "caption")
}}

<!--字符串可以包含换行-->
{{ $msg := `Line one.
Line two.` }}
```

### 变量

在 Hugo 中，每个模板都传递一个 `Page` 。

```html
<!--访问hugo预定义的变量-->
<title>{{ .Title }}</title>

<!--访问自定义变量要使用 $ , 可以使用 = 赋值-->
{{ $address := "123 Main St." }}
{{ $address }}
```

https://gohugo.io/variables/page/

### 函数

https://gohugo.io/functions/

```html
{{ add 1 2 }}
<!-- prints 3 -->


{{ lt 1 2 }}
<!-- prints true (i.e., since 1 is less than 2) -->
```

### Partial

引入其他部分(partial)模板。

```html
{{ partial "<PATH>/<PARTIAL>.<EXTENSION>" . }} 
<!--layouts/partials/header.html-->
{{ partial "header.html" . }}
```

### 逻辑

#### 迭代

```html
{{ range $array }}
    {{ . }} <!-- The . represents an element in $array -->
{{ end }}


{{ range $elem_val := $array }}
{{ $elem_val }}
{{ end }}

<!--For an array or slice-->
{{ range $elem_index, $elem_val := $array }}
{{ $elem_index }} -- {{ $elem_val }}
{{ end }}

<!--判空执行else-->
{{ range $array }}
{{ . }}
{{ else }}
<!-- This is only evaluated if $array is empty -->
{{ end }}
```

#### 条件

```html
<!--with ： if something exists, do this 
    如果不存在或者计算得到0（0，false，空）则跳过-->
{{ with .Params.title }}
<h4>{{ . }}</h4>
{{ end }}

<!--with-else-->
{{ with .Param "description" }}
{{ . }}
{{ else }}
{{ .Summary }}
{{ end }}

<!--if-->
{{ if isset .Params "title" }}
<h4>{{ index .Params "title" }}</h4>
{{ end }}

<!--if-else-->
{{ if (isset .Params "description") }}
{{ index .Params "description" }}
{{ else }}
{{ .Summary }}
{{ end }}

<!--if-elseif-else-->
{{ if (isset .Params "description") }}
{{ index .Params "description" }}
{{ else if (isset .Params "summary") }}
{{ index .Params "summary" }}
{{ else }}
{{ .Summary }}
{{ end }}


<!--and/or-->
{{ if (and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")) }}
```