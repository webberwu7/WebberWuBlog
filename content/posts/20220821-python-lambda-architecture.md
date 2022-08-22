---
title: "Python - 我在aws lambda建立的目錄結構"
date: 2022-08-21T23:55:29+08:00
tags: ['python', 'lambda']
draft: true
---
# 前言
使用aws lambda的服務時，原本是每個lambda獨立開發自己的函示

這樣發現許多關聯相依的lambda都會重複安裝相依的套件，且通用性程式碼無法重複使用

但在 2020年 aws lambda 提供使用image啟動lambda的方式

[aws lambda container image support](https://aws.amazon.com/tw/blogs/aws/new-for-aws-lambda-container-image-support/)

再搭配指定啟動command的方式，可以達到製作一包image提供多個lambda去啟動使用的方法

這篇文主要介紹我目前自己設計使用的結構

# 目錄結構
```
.
├── README.md
├── dockerfile
├── requirements.txt
└── src
    ├── controllers
    │   └── controller.py
    ├── models
    │   └── model.py
    ├── services
    │   └── service.py
    └── test
        └── test_controller.py
```

## 根目錄
主要放跟此專案相關需要的設定

* README.md 
關於此專案的相關介紹，寫下如何啟動他和相關可以使用的指令的方法

* dockerfile
要把code包成一包image所用到的指令

* requirements.txt
所需要的套件安裝清單

## src資料夾
程式開發的程式碼存放的區域

目前有規劃基本的mvc結構

* controllers
主要程式的進入點是controller

controller寫下操作邏輯並製作回傳資料

例如這隻lambda跟user相關就會創立一個 user_controller

* services
將controller商業邏輯拆分成各種動作逐一寫成service

* model
延續service的動作

通常設計是與資料庫連線取得資料或對外打api

* test
如名字一樣就是測試資料夾

會在這裡面寫下unittest的內容

### 情境設計
女友說肚子餓要吃宵夜 (買雞排和珍奶)
```txt
controller 
    -> 買宵夜
service 
    -> 買雞排
    -> 買奶茶
model 
    -> 雞排店 
        -> 買雞排
        -> 買薯條
    -> 手搖杯店 
        -> 買珍奶
        -> 買綠茶
test
    -> controller
        測試是否順利買好宵夜
    -> service
        測試買雞排與買奶茶
    -> model
        測試雞排店是否有開且有供貨
        測試手搖杯店是否有開且有供貨
```