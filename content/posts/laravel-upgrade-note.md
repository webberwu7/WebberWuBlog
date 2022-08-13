---
title: "Laravel 升級之路 - 踩雷筆記"
date: 2022-08-13T16:15:34+08:00
tags: ['laravel']
---
# Laravel 升級之路
隨著laravel版本推陳出新

php 8 也已經於2020年末推出了

之前專案使用的 laravel lts版本是 laravel 6 將在2022.09邁入不再維護的日期

此篇文章是記錄將laravel 6 升級至 9 的過程中所執行的動作或遇到的錯誤

屬於一篇方便下次要再升級時可以回來翻找紀錄的地方

## laravel 6 -> 7
參考連結:https://laravel.com/docs/7.x/upgrade#upgrade-7.0

跟著需求改完composer.json後

執行指令
```bash=
composer update
```

### 第一坑: darkaonline/l5-swagger
需要將darkaonline/l5-swagger 升級至 "7.*"

參考連結:
https://github.com/DarkaOnLine/L5-Swagger/wiki/Installation-&-Configuration

### 第二坑: Script @php artisan package:discover --ansi handling the post-autoload-dump event returned with error code 255

參考連結:
https://stackoverflow.com/questions/50840960/script-php-artisan-packagediscover-handling-the-post-autoload-dump-event-retur

其實laravel文件就有寫清楚了
https://laravel.com/docs/7.x/upgrade#symfony-5-related-upgrades


## laravel 7 -> 8
參考連結:https://laravel.com/docs/8.x/upgrade

跟著需求改完composer.json後

執行指令
```bash=
composer update
```

### 第一坑: swagger失敗 Required @OA\Info() not found

解法原因
https://github.com/zircote/swagger-php/blob/master/docs/guide/faq.md

需要在加入namespace 和 空的class
且註解如果連在一起要連貫 
```php=
 *         @OA\JsonContent(
 *             @OA\Property(property="error", type="string",
 *                 example="Exception"
 *             ),
 *         ),
 *     ),
 * )
 * 
 * 
 *
 * @OA\Post(
 *     path="/api/projects/{projectID}/export/statistical-report",
 *     security={{"Bearer":{}}},
 *     operationId="專案匯出統計報告",
 *     tags={"Export 匯出",},
 *     summary="專案匯出統計報告",
 *     description="後台登入後，專案匯出統
```
不可以這樣做
```php=
 *         @OA\JsonContent(
 *             @OA\Property(property="error", type="string",
 *                 example="Exception"
 *             ),
 *         ),
 *     ),
 * )
 */ 

/**
 * @OA\Post(
 *     path="/api/projects/{projectID}/export/statistical-report",
 *     security={{"Bearer":{}}},
 *     operationId="專案匯出統計報告",
 *     tags={"Export 匯出",},
 *     summary="專案匯出統計報告",
 *     description="後台登入後，專案匯出統
```
## php 7.3 -> 8.0
### 安裝 php 8.0
#### 這裡是mac local安裝php 8
```
brew install php@8.0
```
### zshrc 更新
```
vi ~/.zshrc
```

加入下面這兩行
```
export PATH="/usr/local/opt/php@8.0/bin:$PATH"
export PATH="/usr/local/opt/php@8.0/sbin:$PATH"
```
### 第一坑: kreait/laravel-firebase 需要升級

```
 "kreait/laravel-firebase": "^4.1",
```

### 第二坑: tymon/jwt-auth 不支援php 8
先暫時換成這個
```
 "php-open-source-saver/jwt-auth": "^1.4"
```


## laravel 8 -> 9
### 第一坑: league/flysystem-aws-s3-v3 需要升級
```
"league/flysystem-aws-s3-v3": "^3.0.11",
```

### 第二坑: Undefined constant Illuminate\Http\Request::HEADER_X_FORWARDED_ALL

![](https://i.imgur.com/6Xkf3Qd.png)
