---
title: "关于"
slug: "about"
license: false
comment: false
---

# 关于我

咸鱼 + 懒狗

工作两年半的安卓开发

略微熟悉 kotlin、java、c;
go 和 rust 半入门

# 关于这个博客

博客使用 hugo 生成，托管于 Cloudflare Pages。

<!-- hugo提供了 4 种类型的notice（warning，info，note 和 tip），必要时可以加上。 [^1]

[^1]: [hugo-notice](https://github.com/dtomlinson91/hugo-notice-admonition) -->

## 内容生成

博客 content 结构使用 hugo bundle，由 Front Matter CMS 插件[^1]进行管理，使用方式可以参考[支持Page Bundle的MarkDown编辑器](https://anybook.cc/posts/%E6%94%AF%E6%8C%81page-bundle%E7%9A%84markdown%E7%BC%96%E8%BE%91%E5%99%A8/)

推荐阅读下 markdown 基本规范[^2] 倒是可以学到不少新东西

### 使用到的插件如下：
- Front Matter CMS
- Hugo Snippets
- IME and Cursor
- Markdown All in One
- Markdown Preview Enhanced
- Vim

```json
// 推荐配置如下, .vscode/extensions.json
{
    "recommendations": [
        "eliostruyf.vscode-front-matter",
        "fivethree.vscode-hugo-snippets",
        "beishanyufu.ime-and-cursor",
        "yzhang.markdown-all-in-one",
        "vscodevim.vim",
        "shd101wyy.markdown-preview-enhanced",
        "bierner.emojisense",
    ]
}
```

[^1]:https://marketplace.visualstudio.com/items?itemName=eliostruyf.vscode-front-matter 
[^2]:https://www.markdownguide.org/basic-syntax/