+++
date = '2025-07-07T20:16:23+08:00'
draft = true
title = 'hugo depoly'
+++

# hugo need install weight

1. utyerance评论系统
2. 加音乐播放器：Aplayer  +  MetingJs
3. widgets homepage 小组件
4. opengraph
这部分是用来配置 Open Graph协议 的，它能让你自定义当网站链接被分享到社交媒体（如 Twitter, Facebook, Telegram 等）时，显示的预览卡片样式。

twitter:

site: 在这里填写你的 Twitter 用户名（例如 @hugo_rocks）。当你的文章被分享时，卡片上会显示 "via @你的用户名"。

card: 设置 Twitter 卡片的样式。

summary_large_image: 显示一张大的特色图片，下方是标题和描述（推荐）。

summary: 显示一张小方形图片，标题和描述在右侧

5. defaultImage
这里用来设置一个默认的社交分享图片。

如果某篇文章没有指定自己的特色图片（featured_image），那么当这篇文章被分享时，就会使用这里设置的默认图片。

opengraph:

enabled: false 设为 true 来启用这个功能。

local: false 如果你的默认图片存放在 Hugo 网站的 static 文件夹下，就设为 true。如果图片是通过一个完整的 URL 链接引用的，就设为 false。

src: 填写图片的路径或完整的 URL。

如果 local: true，这里填相对路径，如 images/default-cover.jpg。

如果 local: false，这里填完整 URL，如 https://example.com/images/default-cover.jpg。

6. colorScheme
这部分控制网站的主题颜色模式（白天/黑夜模式）。
toggle: true
设为 true 会在网站上显示一个切换按钮 ☀️/🌙，让访问者可以手动在浅色和深色模式之间切换。
default: auto
设置网站的默认颜色模式。
auto: （推荐） 自动根据访问者操作系统的设置来显示浅色或深色模式。
light: 强制默认显示浅色模式。
dark: 强制默认显示深色模式。




# GitHub action
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