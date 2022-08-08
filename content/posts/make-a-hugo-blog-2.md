---
title: "菜雞部落客的hugo踩雷之路 ( 3 ) - 優化網站"
date: 2022-08-08T23:37:05+08:00
tags: ['hugo']
draft: true
---

# 文章參數配置
相信大家在創建文章時都會看的這樣的文字在最前方

```md
---
title: "菜雞部落客的hugo踩雷之路 ( 3 ) - 優化網站"
date: 2022-08-06T23:37:05+08:00
draft: true
---
```

這是hugo提供給使用者編輯page頁面時可設定的參數區

[REF: <https://gohugo.io/variables/page/#page-level-params>]

目前的理解是，這些參數可以自定義加入進去，然後看layout有沒有支援

那我們就先加入常見主題都支援的 `tags` 吧！

```
tags: ['hugo']
```

就會看到文章中出現 tags 囉！
![image](https://i.imgur.com/9fKI8O5.png)

# Content Organization




