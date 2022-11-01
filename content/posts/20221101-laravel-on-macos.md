---
title: "Laravel on MacOS"
date: 2022-11-01T12:00:38+08:00
tags: [php, laravel]
---
# 前置準備
## HomeBrew
HomeBrew 是 Mac 上好用的套件管理工具

我們可以透過 homebrew 來安裝 laravel 所需要的php版本環境

REF: [https://brew.sh/index_zh-tw](https://brew.sh/index_zh-tw)

在 macOS 的終端機，或 Linux 的 shell 提示貼上這串直接安裝

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## PHP 
* laravel 8.x需要 php 7.3

    `brew install php@7.3`

* laravel 9.x 需要php 8以上

    `brew install php@8.1`

## PHP Composer
composer 是 php 的套件管理工具，想像他跟 pip, npm 是一樣的類型工具

REF: [https://getcomposer.org/](https://getcomposer.org/)

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

裝完後會多出一個 `composer.phar`，我們把它放到可使用指令區

```bash
sudo mkdir /usr/local/bin -p
sudo mv composer.phar /usr/local/bin/composer
```

使用指令 `composer --version` 確認是否有安裝成功
```shell
➜  ~ composer --version
Composer version 2.3.5 2022-04-13 16:43:00
```

# 建立 Laravel
參考 laravel 官方文件建立第一個 laravel 專案

REF: [Laravel安裝](https://laravel.com/docs/9.x/installation#your-first-laravel-project)

1. 使用composer 安裝 laravel
    ```bash
    # composer create-project laravel/laravel {換成你的專案資料夾名稱}
    composer create-project laravel/laravel example-app
    ```

2. 進到專案資料夾
    ```bash
    # cd {專案資料夾名稱}
    cd example-app
    ```

3. 啟動服務
    ```bash
    php artisan serve
    ```

4. 打開瀏覽器確認是否順利啟動
預設網站會在`http://localhost:8000`中找到

![](https://i.imgur.com/SCQTuKV.png)
(這張圖是laravel 8 的初始畫面)


# 建立 laravel 網站所需要的相關服務
這裡會使用docker的服務 docker-compose 快速啟動相關服務

1. 新增一個檔案 `docker-compose.yml`

    加入設定內容
    ```yaml=
    version: '3'
    services:
    redis:
        image: redis
        ports:
        - 6379:6379

    mysql:
        image: mysql:5.7
        command: --default-authentication-plugin=mysql_native_password
        environment:
        - MYSQL_DATABASE=我是資料庫名稱
        - MYSQL_USER=我是使用者名稱
        - MYSQL_PASSWORD=我是使用者密碼
        - MYSQL_ROOT_PASSWORD=我是root密碼
        - TZ=UTC
        volumes:
        - ~/.laravel.pratice/data/mysql:/var/lib/mysql
        ports:
        - 3306:3306

    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        environment:
        - PMA_ARBITRARY=1
        - EXTERNAL_IP=0.0.0.0
        ports:
        - 8080:80
        depends_on:
            - "mysql"

    ```

2. 啟動服務
    ```shell
    docker-compose up
    ```

3. 設定 laravel 環境變數檔案 `.env`
    
    在專案資料夾中執行複製範例檔
    ```bash
    cp .env.example .env
    ```

    接下來在.env中 將資料庫相關的參數設定好

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=資料庫名稱
    DB_USERNAME=帳號
    DB_PASSWORD=密碼
    ```


* 踩雷

    DB_HOST=127.0.0.1

    APP_URL=127.0.0.1

    如果在地端請從localhost改成127.0.0.1

* 關閉服務時 結束docker compose

    `docker-compose down`


# 製做 laravel docker image

1. 寫好 dockerfile

    基底 image 可以使用 dwchiang 製作的 nginx-php-fpm (https://hub.docker.com/r/dwchiang/nginx-php-fpm)

    ```dockerfile
    FROM dwchiang/nginx-php-fpm:8.0.12-fpm-buster-nginx-1.21.1

    # 更新 apt-get 套件包與安裝常見工具
    RUN apt-get update -y && apt-get install -y libmcrypt-dev openssl zip unzip 

    # 安裝php相關擴充套件
    RUN docker-php-ext-install -j$(nproc) \
            bcmath \
            mysqli \
            pdo \
            pdo_mysql \
            exif \
            opcache

    # 下載 composer
    RUN apt-get -y install curl
    RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

    # 將程式碼複製到 /var/www/html 底下
    ADD /{程式碼laravel資料夾} /var/www/html
    # 切換指令執行位置 /var/www/html
    WORKDIR /var/www/html

    # composer 安裝所需php套件
    RUN composer install --prefer-dist
    RUN composer dump-autoload

    # 設定資料夾使用者權限
    RUN chown -R www-data:www-data /var/www/html
    RUN chmod 770 /var/www/html/storage

    # 設定對外 port
    EXPOSE 80
    CMD ["/docker-entrypoint.sh"]
    ```
2. Build image
    
    `docker build -t [image_name] . --no-cache`

3. Run container
    
    `docker run -d --name [container_name] -p 8000:80 [image_name]:latest`

4. API Test
    
    get root page 
    
    `curl 127.0.0.1:8000`