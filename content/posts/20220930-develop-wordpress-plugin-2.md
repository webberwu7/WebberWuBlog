---
title: "Wordpress 插件開發筆記 (3) - PHP Composer 套件管理 / Swagger"
date: 2022-09-30T18:00:00+08:00
tags: ['php', 'wordpress', 'plugin']
---
在wordpress中導入MVC架構後，會以api的方式讓前後端溝通

為了方便前後端溝通順利，需要有一套好的文件系統

常見的api文件就是swagger

但我們總不可能自己寫一套輸出swagger文件的功能

因此好的生態系就會出現開源的套件提供開發者安裝使用

此篇會介紹如何使用php composer 安裝套件 zircote/swagger-php 並匯出 swagger的文件

## Swagger

Swagger is a suite of tools for API developers from SmartBear Software and a former specification upon which the OpenAPI Specification is based. 

[REF:[SWAGGER WIKI](https://en.wikipedia.org/wiki/Swagger_(software))]

### Swagger UI 漂亮的介面
![image](https://static1.smartbear.co/swagger/media/images/tools/opensource/swagger_ui.png?ext=.png)

## PHP Composer
### 安裝 Composer
```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

### 換個名字
```shell
mv composer.phar composer
```

## 安裝 swagger
```shell
composer require zircote/swagger-php
```

### 加上 Docs 資料夾
在app資料夾下加上docs

並在中建立api文件


* Info.php
    ```php
    <?php

    namespace MyPlugin\Docs;

    /**
     * @OA\Info(
     *      version="1.0.0",
     *      title="Wordpress My Plugin API 文件",
     *      description="Wordpress My Plugin API 文件",
     *      @OA\Contact(
     *          email="nmmn1501@gmail.com"
     *      ),
     * )
     */
    class Info
    {
    }
    ```

* Server.php
    ```php
    <?php

    namespace MyPlugin\Docs;

    /**
     * @OA\server(
     *      url = "http://localhost:8000/?rest_route=",
     *      description="本地端"
     * )
     */

    class Server
    {
    }
    ```
* api
    ```php
    <?php

    namespace MyPlugin\Docs\Api;
    /**
    * @OA\Get(
    *     path="/my-plugin/v1/health-check",
    *     operationId="health check",
    *     tags={"health check"},
    *     summary="health check",
    *     description="health check",
    *     @OA\Response(
    *         response=200,
    *         description="plugin health check",
    *     ),
    *     @OA\Response(
    *         response=500,
    *         description="伺服器錯誤，請聯絡工程師",
    *     ),
    * )
    */
    class HealthCheckRoute
    {
    }

    ```

### 輸出swagger yaml
``` shell
# 創建 api-docs資料夾
mkdir api-docs
# 建立匯出swagger文件檔
touch api-docs/api-docs.yaml 
# 執行匯出指令
./vendor/bin/openapi -b autoload.php -o api-docs/api-docs.yaml app/Docs
```

## Visual Studio Code - Swagger Viewer
如果你是使用 visual studio code 開發的人

可以安裝 swagger viewer套件

便可以直接對著產出的swagger文件立即預覽

[https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer](https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer)

![image](https://cdn.rawgit.com/arjun-g/vs-swagger-viewer/master/docs/swagger-context-menu.png)