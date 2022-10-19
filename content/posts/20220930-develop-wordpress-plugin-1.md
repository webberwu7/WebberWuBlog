---
title: "Wordpress 插件開發筆記 (2) - wp option 與 MVC 框架"
date: 2022-09-30T17:23:12+08:00
tags: ['php', 'wordpress', 'plugin']
---
上一篇 我們在wordpress中建立起一個helloworld的插件

並且加入一個後台設定頁面

這篇要描寫如何將可設定的參數加到wordpress中

並且使用 MVC 架構設計API給前端使用

## Wordpress Settings

WordPress 提供了兩個核心 API，使管理界面易於構建、安全並與 WordPress 管理的設計保持一致。

The Settings API focuses on providing a way for developers to create forms and manage form data.

The Options API focuses on managing data using a simple key/value system.

REF: [https://developer.wordpress.org/plugins/settings/](https://developer.wordpress.org/plugins/settings/)

## options api
REF: [https://developer.wordpress.org/plugins/settings/options-api/](https://developer.wordpress.org/plugins/settings/options-api/)
* add_option($optionName);

    在 wp option 建立一組 key 對照 

* get_option($optionName);

    在 wp option 取 key 對照值 

* update_option($optionName, $value);

    在 wp option 更新 key 對照值

* delete_option($optionName)

    在 wp option 移除 key 對照

## RESTful API
在wordpress中加入客製化 RESTful API
https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/


* register_rest_route

register_rest_route( string $namespace, string $route, array $args = array(), bool $override = false ): bool

下面範例加上一個health-check的API

```php
class HealthCheckController
{
    public function __construct()
    {
        $this->registerApiRoutes();
    }

    public function registerApiRoutes()
    {
        add_action('rest_api_init', function () {
            register_rest_route('my-plugin/v1', 'health-check', [
                'methods' => 'GET',
                'callback' => [$this, 'healthCheck'],
            ]);
        });
    }

    public function healthCheck()
    {
        return "success";
    }
}
```

## 實用情境設計
我需要讓使用者設定`認證token`與`專案id`

### Model 
我需要一個Model來管理與建立這些需要的設定欄位

* WpOption.php
```php
<?php

namespace MyPlugin\Models;

class WpOption
{
    // Options in this group 
    public $optionNames = [
        "token", "project"
    ];

    public function __construct()
    {
        $this->register_options();
    }

    private function register_options()
    {
        foreach ($this->optionNames as $optionName) {
            add_option($optionName);
        }
    }

    public function getOptions(): array
    {
        $response = [];
        foreach ($this->optionNames as $optionName) {
            $response[$optionName] = $this->get($optionName);
        }

        return $response;
    }

    public function setOptions($options): bool|array
    {
        foreach ($options as $key => $value) {
            $this->set($key, $value);
        }

        return $this->getOptions();
    }

    public function get($optionName): false|string
    {
        if (in_array($optionName, $this->optionNames)) {
            return get_option($optionName);
        }

        return false;
    }

    public function set($optionName, $value): bool
    {
        if (!in_array($optionName, $this->optionNames)) {
            return false;
        }

        return update_option($optionName, $value);
    }
}
```

### Controller

1. 使用 setting controller 負責將 models 初始化
    * SettingController.php
        ```php
        <?php

        namespace MyPlugin\Controllers;

        use MyPlugin\Models\WpOption;

        class SettingController
        {
            public $wpOption;

            public function __construct()
            {
                $this->wpOption = new WpOption();
            }
        }
        ```

2. 編寫提供前端呼叫的API controller 
    * WpOptionController
        ```php
        <?php

        namespace MyPlugin\Controllers\API;

        use MyPlugin\Models\WpOption;
        use WP_REST_Request;

        class WpOptionController
        {
            protected $wpOption;

            public function __construct()
            {
                $this->wpOption = new WpOption();

                $this->registerApiRoutes();
            }

            public function registerApiRoutes()
            {
                add_action('rest_api_init', function () {
                    register_rest_route('my-plugin/v1/api/backstage', '/setting/options', [
                        'methods' => 'GET',
                        'callback' => [$this, 'getOptions'],
                    ]);
                    register_rest_route('my-plugin/v1/api/backstage', '/setting/options', [
                        'methods' => 'POST',
                        'callback' => [$this, 'updateOptions'],
                        'args' => [
                            'token' => [
                                'required' => false
                            ],
                            'project' => [
                                'required' => false
                            ],
                        ]
                    ]);
                });
            }

            public function getOptions(WP_REST_Request $request)
            {
                $options = $this->wpOption->getOptions();

                return $options;
            }

            public function updateOptions(WP_REST_Request $request)
            {
                // get input params
                $data = $request->get_params();

                // update data
                $options = $this->wpOption->setOptions($data);

                return $options;
            }
        }
        ```
