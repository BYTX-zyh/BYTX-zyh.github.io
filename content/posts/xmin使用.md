+++
title = 'Xmin使用'
date = 2023-10-08T16:19:05+08:00
draft = false
categories = ["博客"]
tags = ['hugo','博客']
toc = true
+++

## 特色

- 根据系统切换黑暗模式

## 安装

Hugo的主题应当位于 `/themes/themeName/` 位置，如果了解 git submodule，推荐使用 submodule：
```bash
cd /path/to/site 
git submodule add https://github.com/BYTX-zyh/hugo-xmin.git themes/xmin
```

如果不使用 submodule 则直接 clone 即可：
```bash
cd /path/to/site
git clone https://github.com/BYTX-zyh/hugo-xmin.git themes/xmin
```

而后在网站的 `hugo.toml` 中添加：
```toml
theme = 'xmin'
```

即可使用主题。

## 配置

hugo的配置文件为站点目录下的`hugo.toml`,但是可能会有部分内容需要修改其他文件，例如对主题文件夹下部分内容修改，如果 fork 了本仓库（或不需要对仓库进行更新），则可直接进行修改，否则应当复制一份进行修改。

即如果要修改 `/themes/xmin/static/fonts.css`，则复制一份到 `/static/fonts.css` 并对其进行修改，因为用户文件的优先级是高于主题文件的。（虽然我建议你直接fork了拿来改，反正我应该不会有啥大更新了）。

### 常见配置

```toml

```

### giscus

[giscus](https://github.com/giscus/giscus)为开源的博客评论框架，利用GitHub discussions，可以实现登录评论与匿名评论。

首先点击[giscus](https://giscus.app/zh-CN) 页面，根据其要求进行设置：
- 公开仓库
- 安装[gitscus](https://github.com/apps/giscus)
- [启用 discussions 功能](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/enabling-or-disabling-github-discussions-for-a-repository)
- 映射关系可选 `Discussion 的标题包含页面的 pathname`
- discussion 分类 按照推荐选择 `announcements`
- 特性、主题可以根据需求自己选择
- 将出现的 script 代码替换到本主题的 `/partials/giscus.html`

在当前版本的代码中，`/partials/giscus.html` 为：
```html

{{ $highlightTheme := "" }}
{{ if .Site.Params.useDarkMode }}
    {{ $highlightTheme = .Site.Params.highlightThemeDark }}
{{ else }}
    {{ $highlightTheme = .Site.Params.highlightThemeLight }}
{{ end }}

<script src="https://giscus.app/client.js"
        data-repo="BYTX-zyh/BYTX-zyh.github.io"
        data-repo-id="R_kgDOKWAiUQ"
        data-category="Announcements"
        data-category-id="DIC_kwDOKWAiUc4CZ4bN"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme=  {{ $.Scratch.Set "highlightTheme" $highlightTheme }}
        data-lang="zh-CN"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>
```

最上面`{{}}` 的部分为模板，用于指示根据系统暗色模式选择的主题，其对应的值设置在`hugo.toml`中：
```toml
[params]
useDarkMode = true # 使用暗色模式
highlightThemeLight = "noborder_light"   # 亮色模式下的高亮主题 不启用的情况下使用 light 主题
highlightThemeDark = "dark_dimmed"     # 暗色模式下的高亮主题
```

将得到的除`data-theme`的部分进行修改，将主题写入对应的配置文件中。




