---
layout: post
#PHP三方管理工具-composer
title:  PHP三方管理工具-composer
#时间配置
date:   2017-06-08 17:44:16 +0800
#大类配置
categories: 环境配置
#小类配置
tag: PHP
---

* content
{:toc}

#### Composer安装
----
1. 下载安装composer（终端命令）
```shell
~$ curl -sS https://getcomposer.org/installer | php
或
~$ php -r "readfile('https://getcomposer.org/installer');" | php
```

2. 将composer移动到bin中，终端就可以直接使用composer的命令了
```shell
~$ mv composer.phar /usr/local/bin/composer
```

#### Composer使用
---
1. 终端进入要添加三方库的项目中<br>
    ![2.png](/styles/images/resources/704AF286BFDA7BFD223666B3D981444A.png)<br><br>
2. 使用命令 ```composer init``` 配置 ```composer.json``` 文件<br>

```
macbookdeMacBook-Pro:test macbook$ composer init
  Welcome to the Composer config generator  
This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [macbook/test]: test
 The package name test is invalid, it should be lowercase and have a vendor name, a forward slash, and a package name, matching: [a-z0-9_.-]+/[a-z0-9_.-]+ 
Package name (<vendor>/<name>) [macbook/test]: test/test
Description []: 这是一个 composer的测试
Author [赵宏亚 <feiliyaailiya1@icloud.com>, n to skip]: 赵宏亚 <feiliyaailiya1@icloud.com>
Minimum Stability []: 
Package Type (e.g. library, project, metapackage, composer-plugin) []: 
License []: 

Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? yes
Search for a package: leancloud/leancloud-sdk
Enter the version constraint to require (or leave blank to use the latest version): v0.5.6
Search for a package: 
Would you like to define your dev dependencies (require-dev) interactively [yes]? 
Search for a package: 

{
    "name": "test/test",
    "description": "这是composer的测试",
    "require": {
        "leancloud/leancloud-sdk": "v0.5.6"
    },
    "authors": [
        {
            "name": "赵宏亚",
            "email": "feiliyaailiya1@icloud.com"
        }
    ]
}

Do you confirm generation [yes]?
```
**也可以直接手动创建composer.json文件直接里边写数据**

#### 查找三方库方法
---
##### 1> 终端模糊查询sdk

```
composer search leancloud/leancloud-sdk
```
![3.png](/styles/images/resources/9789C56C64690B3AF84C1015E720B511.png)

##### 2> 查询sdk版本

```
composer show -all leancloud/leancloud-sdk
```
![4.png](/styles/images/resources/2AB6B7E41E26EEFB765D9A464F1F0D84.png)

##### 3> 生成了composer.json文件

![5.png](/styles/images/resources/32D3E0F287F753B20E859D2740DBDA7D.png)

##### 4> 运行 `composer install` 命令下载sdk,加载完毕
![6.png](/styles/images/resources/98D9CB72BA149F552F2B5B5E649207B7.png)
![7.png](/styles/images/resources/5BCCC0219A70B8A21C2A56D5066D801F.png)

##### 5> 引入头即可使用
```php
namespace LeanCloud;
require "vendor/autoload.php";
use LeanCloud\Client; 
```
![8.png](/styles/images/resources/C94E9DC23855410E14D0718775C44AA1.png)
