---
title: "菜雞部落客的hugo踩雷之路 ( 2 ) - 部署到github上的靜態網站"
date: 2022-08-06T13:50:36+08:00
draft: true
---

# 靜態網站

所謂靜態網頁，便是指超文件標示語言HyperText Markup Language，簡稱：HTML。是一種常與CSS及Javascript配合製作的網站建置基礎技術，網頁以純文字或圖片形式為主，內容固定、互動性差，且無後台資料庫。一般來說只要是網頁副檔名為html或htm，皆可辦定為靜態網頁。因爲靜態網頁的穩定性和無需連接資料庫，因此能快速開啟頁面，也更容易被搜索引擎檢索，所以許多人在編寫網頁時會將動態網頁、影像、動畫等轉變成靜態方式，就是所謂的“偽靜態網頁”，來提高搜尋引擎的友善度進而達到排名優化。

[REF: [上網查到的說明](https://www.bigboycancode.com/website-planning/static-vs-dynamic-website/)]

# GitHub Pages
https://pages.github.com/

GitHub Pages是GitHub提供的一個網頁代管服務，於2008年推出[1][2]。可以用於存放靜態網頁，包括部落格、項目文檔[3][1]甚至整本書。[4]Jekyll軟體可以用於將文檔轉換成靜態網頁，該軟體提供了將網頁上傳到GitHub Pages的功能。[5]一般GitHub Pages的網站使用github.io的子域名，但是用戶也可以使用第三方域名。[6]

[REF: [Wiki](https://zh.wikipedia.org/zh-tw/GitHub_Pages)]

## 1. 先來 github 創建一個 repository

![image](https://i.imgur.com/5jZWfsB.png)

創建後先不要急著把專案上push上來

停在這個頁面一下，我們需要先把hugo的config設定好再來上傳

![image](https://i.imgur.com/IILSoH7.png)

## 2. 設定hugo config baseUrl 至 github page

Github Page的域名規則是這樣

`https://{github帳號}.github.io/{剛剛創建的repository}`

所以我們到config.toml檔設定
```
baseURL = 'https://webberwu7.github.io/WebberWuBlog'
```

設定改好後 hugo 更新一下!
```zsh
hugo
```

## 3. 推專案上 github 囉 
1. 先在hugo專案資料夾 git init 和設定對照的 github remote
    ```zsh
    git init
    git remote add origin git@github.com:webberwu7/WebberWuBlog.git
    ```

2. commit & push
    ```zsh
    git add --all
    git commit -m "init hugo blog"
    git push origin master
    ```