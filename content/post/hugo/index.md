+++
date = '2025-07-07T20:16:23+08:00'
title = 'Hugo Depoly'
draft = false
description = ""
categories = "Hugo"
image = ""
+++

## Hugo学习记录

### 对于main文件夹和theme文件夹的优先级
  
  main文件夹里面的文件优先级最高，如果main中有文件的层级与在theme中一致，那么会优先采取main中的设置。
所以为了防止theme中的源文件被修改，通常会复制一份到main文件中进行修改，这样如果出问题了，还有后悔药吃。





## Hugo optimization 手记

### 修正时间

**存在文章时间显示错误**
*Jul 07, 2025  -> 显示为 Jul 07, 80815*

在netlify.toml文件夹中添加：
```
[context.production]
    command = "cd exampleSite && hugo --gc --themesDir ../.. -b ${URL}"
    [context.production.environment]
        HUGO_ENV = "production"
        TZ = "/usr/share/ZZHAO/PRC"
```

### GitHub action

1. 需要到github账户的developer生成TOKEN（目前是有限期一年）
2. 然后到blog的main仓库的设置添加token并命名为TOKEN，这是因为在本地文件的yaml文件里面的名字是TOKEN，这样可以不改此文件。
在hugo主文件创建一个.github/workflows/xxxx.yaml文件，将以下内容复制进去:
``` yaml
name: deploy

# 代码提交到main分支时触发github action
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
        - name: Checkout
          uses: actions/checkout@v4
          with:
              fetch-depth: 0

        - name: Setup Hugo
          uses: peaceiris/actions-hugo@v3
          with:
              hugo-version: "latest"
              extended: true

        - name: Build Web
          run: hugo -D

        - name: Deploy Web
          uses: peaceiris/actions-gh-pages@v4
          with:
              PERSONAL_TOKEN: ${{ secrets.你的token变量名 }}
              EXTERNAL_REPOSITORY: 你的github名/你的仓库名
              PUBLISH_BRANCH: main
              PUBLISH_DIR: ./public
              commit_message: auto deploy

```

3. 当有新文章后，在本地main推送上github，不是在那个public的文件
4. 推送上去之后，会自动GitHub Actions，修改blog网页的仓库，进而修改个人博客内容。
5. Github action的好处是，不需要在public的文件夹内提交到GitHub上了，只需要在main文件夹即可。


### hugo-stack set double coulnm
  在VSCode里面搜索 ***custom.scss*** 文件，然后添加下面代码即可
```scss
@media (min-width: 1024px) {
  .article-list--compact.links {
      display: grid;
      grid-template-columns: 1fr 1fr;
      background: none;
      box-shadow: none;
      
      article {
          background: var(--card-background);
          border: none;
          box-shadow: var(--shadow-l2);
          margin-bottom: 8px;
          border-radius: 10px;
          &:nth-child(odd) {
              margin-right: 8px;
          }
      }
  }
}
```

### hugo need install weight

1. opengraph
这部分是用来配置 Open Graph协议 的，它能让你自定义当网站链接被分享到社交媒体（如 Twitter, Facebook, Telegram 等）时，显示的预览卡片样式。
twitter:
site: 在这里填写你的 Twitter 用户名（例如 @hugo_rocks）。当你的文章被分享时，卡片上会显示 "via @你的用户名"。
card: 设置 Twitter 卡片的样式。
summary_large_image: 显示一张大的特色图片，下方是标题和描述（推荐）。
summary: 显示一张小方形图片，标题和描述在右侧
2. defaultImage
这里用来设置一个默认的社交分享图片。
如果某篇文章没有指定自己的特色图片（featured_image），那么当这篇文章被分享时，就会使用这里设置的默认图片。
opengraph:
enabled: false 设为 true 来启用这个功能。
local: false 如果你的默认图片存放在 Hugo 网站的 static 文件夹下，就设为 true。如果图片是通过一个完整的 URL 链接引用的，就设为 false：
src: 填写图片的路径或完整的 URL。
如果 local: true，这里填相对路径，如 images/default-cover.jpg。
如果 local: false，这里填完整 URL，如 https://example.com/images/default-cover.jpg。

### 剔除中英文模式，只留下一种

将整个Blog风格统一化，左侧边栏使用英文标注各个page

### 6

6

### 改善白天模式可读性

在Stack以及很多主题中，主题文件夹下的assets/scss下都提供了一个供用户自定义的custom.scss文件。
将下面代码块复制到custom.scss文件中，会对比度会更高，看得会更清晰。
``` scss
:root {
  --body-background: #EBEBEB;
  --accent-color: #1B365D;
  --accent-color-darker: #202A44;
  --accent-color-text: #FFF;
  --body-text-color: #202A44;
}

:root {
  --card-background: #FFF;
  --card-background-selected: #EBEBEB;
  --card-text-color-main: #202A44;
  --card-text-color-secondary: #53565A;
  --card-text-color-tertiary: #888B8D;
}
```

References.     
[oxidane-uni的博客](https://oxidane-uni.github.io/p/%E4%BD%BF%E7%94%A8-hugo-%E5%AF%B9%E5%8D%9A%E5%AE%A2%E7%9A%84%E9%87%8D%E5%BB%BA%E4%B8%8E-stack-%E4%B8%BB%E9%A2%98%E4%BC%98%E5%8C%96%E8%AE%B0%E5%BD%95/#%E6%96%B0%E5%BB%BA%E7%BD%91%E7%AB%99)

### 更改白天/黑夜模式图标

观察网页的CSS可以发现：深色模式下图标的切换就是“一个显示，一个隐藏”，在相关文件中指定该用的图标即可。
在[tabler](https://tabler-icons.io/)下载"sun-high"和"moon-stars"这两个图标，好看。
作者将黑白开关做在了侧边栏里，因而直接在有关的
.\assets\scss\partials\sidebar.scss和\layouts\partials\sidebar\left.html里指定图标即可: 

(1) 在**.\assets\scss\partials\sidebar.scss**中，修改代码为下面这样：
```scss
[data-scheme="dark"] {
    #dark-mode-toggle {
        color: var(--accent-color);
        font-weight: 700;

        .icon-tabler-sun-high {
            display: none;
        }

        .icon-tabler-moon-stars {
            display: unset;
        }
    }
}

#dark-mode-toggle {
    margin-top: auto;
    color: var(--body-text-color);
    display: flex;
    align-items: center;
    cursor: pointer;
    gap: var(--menu-icon-separation);

    .icon-tabler-moon-stars {
        display: none;
    }
}
```

(2)在**\layouts\partials\sidebar\left.html**中，修改代码为下面这样，用于修改图标

```html
{{ if (default false .Site.Params.colorScheme.toggle) }}
    <li id="dark-mode-toggle">
        {{ partial "helper/icon" "sun-high" }}
        {{ partial "helper/icon" "moon-stars" }}
        <span>{{ T "darkMode" }}</span>
    </li>
{{ end }}
```

***(3)修改白天/黑夜模式的文字***

\i18n\zh-cn.yaml中修改即可

## EDN.hugo 优化

1. ~~时间格式format为jul 7, 2025~~
2. ~~将link显示改成2栏~~
3. ~~设置自己的blog头像、网页小图标~~
4. ~~设置Copyright © 2025 etherealzoom~~
5. ~~剔除中英文模式，留中文模式作为默认，后续自己添加内容~~    
about部分可以中英文，一式两份
6. ~~保留默认colorScheme，白天/黑夜模式~~
7. ~~改善白天模式的可读性~~
8. 更改白天/黑夜模式图标
9. ~~新建一个repository作为图床~~
10. ~~开启GitHub Actions~~
11. 评论系统推荐giscus, better than utyerance评论系统
12. 加音乐播放器：Aplayer  +  MetingJs(优先级很低)
13. 
