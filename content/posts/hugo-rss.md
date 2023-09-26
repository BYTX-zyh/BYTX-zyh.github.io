+++
title = 'Hugo Rss'
date = 2023-09-26T19:31:35+08:00
draft = false
categories = ['博客']
tags = ['hugo','rss','博客']
toc = true
+++

本文简介如何为 hugo 博客添加 rss 。

hugo 具有自带的rss 功能[^1]，在 `hugo.toml` 中添加如下代码，启用hugo的rss生成。

```toml
[outputs]
    home = ['html', 'rss']
    section = ['html', 'rss']
    taxonomy = ['html']
    term = ['html']

# 以下为版权声明与作者信息，请自行更改对应字符串
copyright = "©版权信息"
languageCode = "zh-cn"
[Author]
name = "YOUR_NAME"
email = "me@example.com"
```

而后在`layouts/partials/header.html` 的 `<head>` 标签中添加如下字段：
```html
  <link rel="alternate"
          type="application/rss+xml"
          href="{{.Site.BaseURL }}/index.xml"
          title="{{ .Site.Title }}">
```

此时浏览器的rss插件即可找到rss订阅链接。


对于默认生成的rss，其只生成摘要，而且默认监视所有页面的变化，如果需要生成全部内容的rss，或者修改为监视` content\posts`下文章的变化，请参考此处[^2]。

如果对修改内容不感兴趣，只需要修改后的代码，请查看[我的代码](https://github.com/BYTX-zyh/hugo-xmin/blob/master/layouts/_default/rss.xml)，如果需要恢复原样，请删除`rss.xml`[^3]。


[^1]: https://gohugo.io/templates/rss/
[^2]: https://digitaldrummerj.me/hugo-create-rss-feed/ 
[^3]: https://github.com/gohugoio/hugo/blob/master/tpl/tplimpl/embedded/templates/_default/rss.xml