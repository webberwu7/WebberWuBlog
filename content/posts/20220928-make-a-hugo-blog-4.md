---
title: "菜雞部落客的hugo踩雷之路 ( 5 ) - 改變主題"
date: 2022-09-28T17:42:47+08:00
tags: ['hugo']
draft: true
---
在原本使用的 m10c 主題色系較為灰暗

然後我又想加上搜尋功能於是就換成了今天的主題 hugo-theme-pure

## 下載新的目標主題
`git submodule add git@github.com:webberwu7/hugo-theme-pure.git themes/pure`

## 移除舊的主題
`git rm themes/m10c`

## 變更 config.toml 設定