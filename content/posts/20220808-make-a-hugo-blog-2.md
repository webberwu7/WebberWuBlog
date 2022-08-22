---
title: "菜雞部落客的hugo踩雷之路 ( 3 ) - Content 與 layout 的愛恨情仇"
date: 2022-08-08T23:37:05+08:00
tags: ['hugo']
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

hugo的 content有自己一套的組織規則

## Content Sections
首先先來認識 Content Sections

[REF: [Content Sections](https://gohugo.io/content-management/sections/)]

### section有兩個定義
1. 在content下面第一層的資料夾

2. 資料夾裡面有 _index.md

參考下方結構圖
```
content
└── blog        <-- Section, 在content下面第一層的資料夾/
    ├── funny-cats
    │   ├── mypost.md
    │   └── kittens         <-- Section, 因為這個資料夾裡面有 _index.md
    │       └── _index.md
    └── tech                <-- Section, 因為這個資料夾裡面有 _index.md
        └── _index.md
```

### section 特色
* hugo 會自動幫你把content下第一層 section 套用 list的模板

* Content Section vs Content Type

    使用section後hugo會自動把文章的section當成他預設的 content type

    例如說下方結構圖中

    post-1的content type就是 post
    ```
    content
    └── post        <-- Section, 在content下面第一層的資料夾/
        └── post-1.md 
    ```

* Section with Archetypes

    section 特性可以搭派 hugo archetypes 使在特定section中建立的文章

    可以自動對照到相對應的文章樣板

    例如下方結構圖中，在archetypes中建立post資料夾與底下的default.md
    
    如此一來當你在post section建立文章時，他就會自動帶入該樣板了
    ```
    archetypes
    └── post
        └── default.md
    ```
## Content Type
在 section 有提到會依據 section 自動設定文章 type 

那如果這篇文章沒有沒有辦法自動帶入，也沒有手動設定的時候

會有個預設值= page

```
content
└── post     
│   └── post-1.md
└── about.md    <-- 在content下，但沒有被放進任何資料夾中 (且文章中也沒有設定type)
```

# Layout設計

上面講了content的一些資訊究竟是要拿來做什麼的呢？

這就要講到hugo layout

hugo 提供我們設計layout模板渲染生成靜態html檔

我們可以在layout中使用剛剛提到的 section 作為參數帶入 layout中

(這邊我的理解是類似使用php blade寫網頁時在html帶入程式碼和參數，之後一次性產生靜態html檔)

(不專業前端知識如有錯誤，請不要吝嗇告知修正)

所以我們可以看一下 [踩雷之路 (1)]({{< ref "/posts/20220806-make-a-hugo-blog" >}}) 所使用的theme是怎麼寫的吧！

首先先來看這個檔案 `m10c/layouts/_default/list.html`

```html
{{ define "main" }}
  <article>
    <h1>{{ .Title }}</h1>
    <ul class="posts-list">
      {{ range where .Paginator.Pages "Type" "!=" "page" }}
        <li class="posts-list-item">
          <a class="posts-list-item-title" href="{{ .Permalink }}">{{ .Title }}</a>
          <span class="posts-list-item-description">
            {{ partial "icon.html" (dict "ctx" $ "name" "calendar") }}
            {{ .PublishDate.Format "Jan 2, 2006" }}
            -
            {{ partial "icon.html" (dict "ctx" $ "name" "clock") }}
            {{ .ReadingTime }} min read
          </span>
        </li>
      {{ end }}
    </ul>
    {{ partial "pagination.html" $ }}
  </article>
{{ end }}

```

這份list.html中 `{{ range where .Paginator.Pages "Type" "!=" "page" }}`

將所有 type != page 的文章做一個foreach的迴圈

回到首頁 或是`/posts`頁就能看到文章列囉

![image](https://imgur.com/YbxzTlO.png)