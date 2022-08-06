---
title: "菜雞部落客的hugo踩雷之路 ( 1 ) - 建立hugo專案與新增文章"
date: 2022-08-06T11:56:21+08:00
---
# 什麼是 Hugo
Hugo 是一個用 Go 編寫的靜態網站生成器，2013由 Steve Francia 原創，自 v0.14 (2015年) 由 Bjørn Erik Pedersen 主力開發[2]，並由全球各地的開發者和使用者提交貢獻。Hugo 以 Apache License 2.0 授權的開放原始碼專案。[3]
Hugo 一般只需幾秒鐘就能生成一個網站（每頁少於 1 毫秒），被稱為「世界上最快的網站構建框架」，也使 Hugo 大受歡迎，成為最熱門的靜態網站生成器之一，被廣泛採用。
[REF: [wiki](https://zh.wikipedia.org/zh-tw/Hugo_(%E8%BB%9F%E4%BB%B6))]

所以如果我們的網站類型是靜態網站的話，非常推薦使用hugo來建置

## 1. 安裝Hugo
我這裡用的環境是 mac 且使用brew套件管理工具

```zsh
brew install hugo
```

安裝後可以檢查一下版本 確認是否順利完成
```zsh
hugo version
```

hugo version 結果圖
```
➜  ~ hugo version
hugo v0.101.0+extended darwin/amd64 BuildDate=unknown
```

## 2. 開啟一個hugo專案
安裝好後可以使用方便的 `hugo` 指令建立一個預設的 hugo網站
```zsh
# hugo new site {資料夾名稱}
hugo new site webberwu_hugo
```

可以看到新增完後會在資料夾中長出這些東西
```
├── archetypes
├── content
├── data
├── layouts
├── public
│   ├── categories
│   ├── css
│   ├── make-a-hugo-blog
│   ├── page
│   └── tags
├── resources
│   └── _gen
├── static
└── themes
    └── hugo-theme-m10c # 下一步才會安裝的主題 剛新建時會是空的
```


## 3. 安裝漂亮的主題包
可以到這裡 https://themes.gohugo.io/

選擇想要使用的主題進行安裝

此篇文章要使用的是blog類型的

目前使用這個 簡單風格的 m10c 主題

https://themes.gohugo.io/themes/hugo-theme-m10c/

```zsh
# 進到hugo專案的資料夾中
cd [path]
# 使用 git clone 下載安裝
git clone https://github.com/vaga/hugo-theme-m10c.git themes/m10c
```

接下來要到專案資料夾中的 config.toml

加入`theme=m10c`

代表我們現在要使用的主題是在themes中的哪個資料夾名稱

config.toml 結果圖
```
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'


theme = "m10c"
```
---
### 踩雷經驗 - git submodule
如果你要將專案部署上github page的時候

沒有使用git submodule會使build過程失敗

因此我們要在 專案資料夾中使用submodule

新建一個檔案 `.gitmodules`

並在裡面寫下
```
[submodule "themes/m10c"]
	path = themes/m10c
	url = git@github.com:vaga/hugo-theme-m10c.git
```
---
## 4. 完成後就來試跑看看囉！
hugo 提供快速啟動網站伺服器的服務
不需要像傳統網站需要準備好apache或nginx等http伺服器
只需要簡單的打下指令 `hugo server` 就能啟動服務了
```zsh
# -D 是為了後面創建文章時要顯示草稿的頁面
hugo server -D
```

開個瀏覽器 看看畫面吧 http://localhost:1313

![image](https://imgur.com/RCfDOYR.png)


## 5. 開始寫文章吧
可以使用 hugo指令 快速產生基礎的hugo文章頁
```zsh
hugo new make-a-hugo-blog.md
```

你會看到他幫你在content資料夾中新增一個檔案
`make-a-hugo-blog.md`

內容如下
```
---
title: "Make a Hugo Blog"
date: 2022-08-06T13:20:02+08:00
draft: true
---
```

* draft: true 
    代表這篇文章是草稿，如果啟動 hugo server 沒有加上 -d 的參數就不會顯示出來

接下來我們在這個檔案中
用markdown的語法寫下文章的內容
例如說目前正在看的這篇
```
---
title: "菜雞hugo踩雷之路"
date: 2022-08-06T11:56:21+08:00
draft: true
---
# 什麼是 Hugo
Hugo 是一個用 Go ...

所以如果我們的網站類型是靜態網站的話，非常推薦使用hugo來建置

## 1. 安裝Hugo
我這裡用的環境是 mac 且使用brew套件管理工具
.
.
.
```

* hugo server 即時查看結果

    編寫的過程中可以從剛剛啟動的 hugo server服務

    `http://localhost:1313/{文章的檔案名稱}`

    `http://localhost:1313/make-a-hugo-blog/`

---
### 踩雷經驗 - hugo publishDir 調整
再輸入指令hugo產生靜態檔案之前

如果你未來想將網站發佈到github page的話

需要在 config.toml 多加一個設定
```toml
publishDir = 'docs'
```

hugo預設的靜態檔資料夾是在`public`資料夾

但是github page 預設讀取的資料夾名稱是 `docs`

---

內容寫完也確認無誤後

輸入 `hugo` 指令編譯產生靜態檔
```zsh
hugo
```
你就會在 public資料夾中看到你新增的頁面囉 (如果有設定publishDir='docs' 則會在docs資料夾中看到)

## 下集待續
下一集將實作Hugo上傳至github靜態頁面


# 參考文章
https://blog.10oz.tw/20200124-make-a-hugo-blog/


# 其他筆記
## tree 顯示漂亮的資料夾結構
```zsh
brew install tree
```
基本使用方法
```zsh
# -d 顯示目前目錄下所有dir名稱
tree -d 

# -L 往下找幾層
tree -d -L 2
```