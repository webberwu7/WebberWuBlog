---
title: "wordpress 插件開發筆記 (1) - 第一個插件"
date: 2022-09-15T23:38:14+08:00
tags: ['php', 'wordpress', 'plugin']
---
## 1. 基本結構

建立一個資料夾

並在裡面新增一個與資料夾同名的.php檔

做為插件的進入點

```shell
my-plugin
└── my-plugin.php
```

在my-plugin.php加上插件的註解後

wordpress就能讀到該插件了

```php
<?php

/**
 * my-plugin
 * 
 * Plugin Name: my-plugin
 * Description: 學習開發wordpress插件
 * Version:     0.1
 * Author:      webber.wu
 */

```

![](https://i.imgur.com/cpq1ZxR.png)

## 2. 使用autoload與namespace
隨著程式的開發未來檔案數量會越來越多

開發引用其他php就需要一堆require

又可能會遇到撞名的狀況

因此namespace與autoload的搭配就能讓code變成漂亮許多

1. 資料夾中新增autoload.php
    ```php
    <?php

    spl_autoload_register(function ($class) {
        $prefix = 'MyPlugin';

        $base_dir = __DIR__ . '/app/';

        $len = strlen($prefix);
        if (strncmp($prefix, $class, $len) !== 0) {
            // no, move to the next registered autoloader
            return;
        }

        // get the relative class name
        $relative_class = substr($class, $len);

        // replace the namespace prefix with the base directory, replace namespace
        // separators with directory separators in the relative class name, append
        // with .php
        $file = $base_dir . str_replace('\\', '/', $relative_class) . '.php';

        // if the file exists, require it
        if (file_exists($file)) {
            require_once $file;
        }
    });
    ```

    * 第4行 `$prefix = 'MyPlugin';`
        代表這個專案使用的前綴名稱是什麼
    * 第6行 `$base_dir = __DIR__ . '/app/';`
        代表要autoload讀取的資料夾

2. 回到my-plugin.php 加上require
    ```php
    // autoload class object
    require __DIR__ . "/autoload.php";
    ```

#### 目前資料夾結構
```shell
my-plugin
├── autoload.php
└── my-plugin.php
```


## 3. 在app資料夾中新增插件類別 
由於上面autoload寫我們要讀取app資料夾中的物件

所以我們需要在插件資料夾中新增app資料夾

並在app資料夾中新增MyPlugin.php

```php
<?php

namespace MyPlugin;

class MyPlugin
{
    public function __construct()
    {
    }
    
    public function plugin_init()
    {
    }
}

```

#### 目前資料夾結構
```shell
my-plugin
├── app
│    └── MyPlugin.php
├── autoload.php
└── my-plugin.php
```

## 4. 回到wordpress plugin寫下初始化物件
使用wordpress hook : plugins_loaded

作為觸發初始化插件的進入點
```php
use MyPlugin\MyPlugin;

// 讀取這個插件
add_action(
    'plugins_loaded',
    function () {
        $plugin = new MyPlugin();
        $plugin->plugin_init();
        return $plugin;
    }
);
```

## 5. 建立後台選單
接下來我們使用mvc的設計來切分每個功能的物件

1. 新增一個Controllers資料夾並且在裡面加上AdminPageController
    負責管理後台頁面的事件
    ```php
    <?php

    namespace MyPlugin\Controllers;

    class AdminPageController
    {
        public function __construct()
        {
                add_action('admin_menu', function () {
                // 建立選單
                add_menu_page(
                    '我的插件',
                    '我的插件',
                    'manage_options',
                    'my-plugin-options',
                    '',
                    'dashicons-email-alt',
                    99
                );
            });
        }
    }
    ```
2. 回到MyPlugin.php中將controller引入
    ```php
    use MyPlugin\Controllers\AdminPageController;

    class MyPlugin
    {
        protected $adminPageController;

        public function __construct()
        {
            $this->adminPageController = new AdminPageController();
        }

        public function plugin_init()
        {
        }
    }
    ```

登入wordpress後台將外掛啟用

我的插件按鈕就出現了!!

![](https://i.imgur.com/LilUBLO.png)


### 介紹一下 wp 的 add_menu_page 函式的細節
https://developer.wordpress.org/reference/functions/add_menu_page/

```php
add_menu_page( 
    string $page_title,        // 點進該頁面所顯示的標題
    string $menu_title,        // 在選單目錄所顯示的標題
    string $capability,        // wp權限需求
    string $menu_slug,         // wp目錄的客製化網址設定 http://{你的網站}/wp-admin/{menu_slug}
    callable $callback = '',   // 點擊後觸發的callback
    string $icon_url = '',     // 顯示在目錄上的icon
    int|float $position = null // 目錄的座標位置參考上方連結詳細說明
)

add_menu_page(
    '我的插件',
    '我的插件',
    'manage_options',
    'my-plugin-options',
    '',
    'dashicons-email-alt',
    99
);
```

#### 目前資料夾結構
```shell
my-plugin
├── app
│   ├── Controllers
│   │   └── AdminPageController.php
│   └── MyPlugin.php
├── autoload.php
└── my-plugin.php
```

## 6. 定義方便使用的絕對路徑與網址
在my-plugin.php中加上define

這邊定義了插件的絕對網址與插件的絕對路徑

```php
// define absolute url
defined('MY_PLUGIN_ABS_PLUGIN_URL') || define('MY_PLUGIN_ABS_PLUGIN_URL', plugin_dir_url(__FILE__));
defined('MY_PLUGIN_ABS_PLUGIN_PATH') || define('MY_PLUGIN_ABS_PLUGIN_PATH', plugin_dir_path(__FILE__));
```


## 7. 後台插件畫面
有了目錄按鈕後直接點下去會出現404查無此頁

因此我們現在要在這個按鈕加上callback

```php
add_action('admin_menu', function () {
    // 建立主選單
    add_menu_page(
        '我的插件',
        '我的插件',
        'manage_options',
        'my-plugin-options',
        function () {
            $myPluginPagePath = MY_PLUGIN_ABS_PLUGIN_PATH . "public/admin/my-plugin-admin-page.php";
            load_template($myPluginPagePath);
        },
        'dashicons-email-alt',
        99
    );
});
```

callback的function使用load_template的方式前後端分離

可以使前端專心編寫指定路徑上的檔案即可

不會讓controller中混雜 html js 等程式

透過上方的code可以看到需要讀取`插件資料夾/public/admin` 中的 `my-plugin-admin-page.php`

```html
<!DOCTYPE html>
<html lang="en">

<body>
    <h1>MY PLUGIN PAGE<h1>
    <h1>hello world!!<h1>
</body>

</html>
```

![](https://i.imgur.com/ihEdrDI.png)
