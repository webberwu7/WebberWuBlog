---
title: "Wordpress 開發環境"
date: 2022-09-16T11:25:16+08:00
tags: ['wordpress', 'docker']
---
# Wordpress 

WordPress是一個以PHP和MySQL為平台的自由開源的部落格軟體和內容管理系統

WordPress具有外掛程式架構和模板系統

方便使用者開發客製化功能外掛和精美的主題模板

那麼如果要開發外掛或主題，就需要一個wordpress環境

今天這篇會紀錄如何在 Mac 系統中使用 docker 建立一個地端的 wordpress

## 使用 docker-compose 一次將需要的服務啟動
如上面所描述的 wordpress 是以 php 和 mysql 為平台建置而成的

因此我們需要一個 mysql服務 和一個有php的網站建設

* mysql服務
    使用官方提供的 mysql:5.7 

    並且設定好連線的使用者帳號密碼與建立資料庫

* wordpress 服務
    使用官方提供的 wordpress:6.0.0-php8.0-apache    ([REF: wordpress docker hub](https://hub.docker.com/_/wordpress))

    未來如果 wordpress 要升級，可參考官方 tag 改成目標版本號

    php 版本目前使用 8.0 如果未來要升級，可參考官方 tag 改成目標版本號

```yml
version: '3.3'

services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      - MYSQL_DATABASE=wordpress-example
      - MYSQL_USER=wpExampleUser
      - MYSQL_PASSWORD=KhcwtBvD5ygAu
      - MYSQL_ROOT_PASSWORD=ybcM8Wrhr4MqX9ZpsWkQgdGH4NMghNcq
      - TZ=UTC
    ports:
      - 33060:3306
    volumes:
      - ./.db/data/mysql:/var/lib/mysql

  wordpress:
    depends_on:
      - db
    image: wordpress:6.0.0-php8.0-apache
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_NAME: wordpress-example
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpExampleUser
      WORDPRESS_DB_PASSWORD: KhcwtBvD5ygAu
    volumes:
      - ./.wordpress:/var/www/html
```

寫好docker-compose後下指令啟動 
```
docker-compose up
```
如果順利啟動的話就會看到 wordpress 的引導安裝介面了

![image](https://imgur.com/O2193uv.png)

![image](https://imgur.com/lTYDKjX.png)

安裝完成登入後，就能看到wordpress的後台介面囉

![image](https://imgur.com/LCTH28q.png)


## 將開發的插件放入目前運行中的 wordpress 
在docker-compose中有使用 volume 功能

所以可以將docker容器中的位置對照出來外面

設定是放在目前位置下的 .wordpress 資料夾中

```yaml
    volumes:
      - ./.wordpress:/var/www/html
```

目前的資料夾結構
```txt
.
├── db
│   └── data
├── wordpress
│   ├── wp-admin
│   ├── wp-content
│   ├── wp-includes
│   ├── index.php
│   ├── license.txt
│   ├── readme.html
│   ├── wp-activate.php
│   ├── wp-blog-header.php
│   ├── wp-comments-post.php
│   ├── wp-config-docker.php
│   ├── wp-config-sample.php
│   ├── wp-config.php
│   ├── wp-cron.php
│   ├── wp-links-opml.php
│   ├── wp-load.php
│   ├── wp-login.php
│   ├── wp-mail.php
│   ├── wp-settings.php
│   ├── wp-signup.php
│   ├── wp-trackback.php
│   └── xmlrpc.php
├── LICENSE
├── README.md
└── docker-compose.yml
```

wordpress 的插件會安裝在 wp-content -> plugins 中

所以將開發的插件複製到這個位置他就會同步至 wordpress 然後自動讀取

例如： 我們可以將 [wordpress 插件開發筆記 (1) - 第一個插件](/WebberWuBlog/posts/202220915-develop-wordpress-plugin) 開發的插件加入其中

```txt
├── wordpress
    ├── wp-admin
    └── wp-content
        └── plugins 
            └── my-plugin 
```

就可以看到wordpress的插件列表中出現我們自行開發的插件了

![image](https://imgur.com/DyS7djk.png)


## WP-CLI
WP-CLI是 WordPress 的官方命令行界面。
它可以讓你從終端機的介面下指令來快速管理 WordPress網站 、網站主題、網站插件等等。
這通常是當你 WordPress網站設計 玩到一定的程度之後會開始摸索研究的軟體，讓你晉升高級 WordPress網頁設計師，更便捷快速的管理你的 WordPress網站。

REF: https://raise-up.com.tw/wordpress-tutorial/wp-cli-introduction.html/


可是使用了 docker-compose 要如何自動執行wp-cli呢？

因此要在 `docker-compose.yaml` 中加入 wordpress-cli 的服務去執行他

```yaml
  wordpress-cli:
    depends_on:
      - db
      - wordpress
    user: "33:33"
    image: wordpress:cli
    environment:
      WORDPRESS_DB_NAME: wordpress-example
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpExampleUser
      WORDPRESS_DB_PASSWORD: KhcwtBvD5ygAu
    command: >
      /bin/sh -c '
      sleep 10;
      wp core install --path=/var/www/html \
                      --url="http://localhost:8000" \
                      --title="wordpress-example" \
                      --admin_user=wpExampleUser \
                      --admin_password=wpExampleUser \
                      --admin_email=wp.example@example.com;
      wp theme install colormag --activate --allow-root;
      wp plugin install aryo-activity-log --activate --allow-root;
      '
    volumes:
      - ./.wordpress:/var/www/html
```

這裡我簡單寫下兩個指令
1. wp core install   : 安裝wordpress 並設定網址,標題和管理員等資料
2. wp theme install  : 安裝wordpress主題
3. wp plugin install : 安裝wordpress插件