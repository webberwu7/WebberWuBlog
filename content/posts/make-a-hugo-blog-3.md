---
title: "菜雞部落客的hugo踩雷之路 ( 4 ) - 終於有頭像了"
date: 2022-08-16T12:19:14+08:00
tags: ['hugo']
---

# 部落格作者頭像

在目前使用的 m10c 主題中有提到設定

---
### Configuration

In your `config.toml` file, define the following variables in `params`:

- `author`: 作者的名字
- `description`: 關於作者的簡單的敘述
- `avatar`: 放置作者大頭照的路徑
- `menu_item_separator`: 在目錄類型之間的分隔符號. HTML allowed (default: " - ")
- `favicon`: 使用favicon.ico的絕對路境 (default: "/favicon.ico")

---

根據hugo的目錄設計

可以將靜態資源放在 static 資料夾中 ([REF: Hugo Static Files](https://gohugo.io/content-management/static-files/))

By default, the static/ directory in the site project is used for all static files (e.g. stylesheets, JavaScript, images). 

The static files are served on the site root path 

(eg. if you have the file static/image.png you can access it using http://{server-url}/image.png, to include it in a document you can use `![Example image](/image.png)` ).

## 實作囉
將我的頭像圖片放到 static/images/avatar.jpg

然後在config.toml中加入這些設定

```toml
[params]
    author = 'webber'
    avatar = "images/avatar.jpg"
    description = '目前是一名後端工程師'
```