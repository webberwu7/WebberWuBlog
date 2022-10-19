---
title: "20221006 Develop Wordpress Plugin Unittest"
date: 2022-10-06T00:24:41+08:00
tags: []
draft: true
---

## PHP UNIT TEST
### 安裝 phpunit
```shell=
composer require --dev phpunit/phpunit
```
### 設定 phpunit.xml
```xml=
<?xml version="1.0" encoding="UTF-8"?>
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" backupGlobals="false" backupStaticAttributes="false" colors="true" convertErrorsToExceptions="true" convertNoticesToExceptions="true" convertWarningsToExceptions="true" processIsolation="false" stopOnFailure="false" xsi:noNamespaceSchemaLocation="https://schema.phpunit.de/9.3/phpunit.xsd">
  <coverage>
    <include>
      <directory suffix=".php">app</directory>
    </include>
    <report>
      <text outputFile="php://stdout"/>
    </report>
  </coverage>
  <testsuites>
    <testsuite name="plugin-test">
      <directory suffix="Test.php">tests</directory>
    </testsuite>
  </testsuites>
  <logging/>
</phpunit>
```

### 第一個測試
在專案中加入test資料夾加上HelloTest.php
```php=
<?php

namespace MyPlugin\tests;

use PHPUnit\Framework\TestCase;

class HelloTest extends TestCase
{
    public function testBase()
    {
        $this->assertEquals(2, 1 + 1);
    }
}
```

### RUN TEST
```shell
./vendor/bin/phpunit
```