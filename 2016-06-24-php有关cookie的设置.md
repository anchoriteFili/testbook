---
layout: post
#php有关cookie的设置
title:  php有关cookie的设置
#时间配置
date:   2016-06-24 17:21:50 +0800
#大类配置
categories: 知识
#小类配置
tag: php
---

* content
{:toc}

##### 1. 加密并设置cookie

**注意：使用setcookie时<?php?>必须紧贴顶部**

```php
<?php 
// 添加cookie部分
// 定义变量，进行赋值
$zhanghao = $mima = '';
$zhanghaoErr = $mimaErr = '';

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    
    if (empty($_POST['zhanghaophp'])) {
        
        $zhanghaoErr = '账号不能为空';
    } elseif (empty($_POST['mimaphp'])) {
        
        $mimaErr = '密码不能为空';
    } elseif (!empty($_POST['zhanghaophp']) && !empty($_POST['mimaphp'])) {
        
        $zhanghao = test_input($_POST['zhanghaophp']);
        $mima = test_input($_POST['mimaphp']);
        // 进行两次加密
        // base64加密
        $cookie = 'username=' . $zhanghao . ',password=' . $mima;
    
        $str1 = base64_encode($cookie);
        // echo '一次加密：' . $str1 . '<br>';
    
        $str2 = 'ZHY' . $str1;
        $str3 = base64_encode($str2);
        // echo '二次加密：' . $str3 . '<br>';
    
        // 创建cookie
        $expire = time() + 60*60*24*30;
        setcookie('gonghuizhudi', $str3,  $expire);
        
        // echo '创建了cookie';
    }
    
    // echo '来到了cookie';
}

function test_input($data) {
    $data = trim($data);
    $data = stripslashes($data);
    $data = htmlspecialchars($data);
    return $data;
}
?>
```

##### 2. 获取cookie并解密

```php
<?php 
/*
 * 我要在这里进行一大堆的判断，就是这个样子
 * */

// 获取cookie
if (isset($_COOKIE['gonghuizhudi'])) {
    
    $str1 = $_COOKIE['gonghuizhudi'];
    
    $str2 = base64_decode($str1);
    
    $str3 = substr($str2, 3);
    $str4 = base64_decode($str3);
    // echo '二次解密：' . $str4 . '<br>';
    
    $zhanghao = substr($str4, 9, strpos($str4, ',') - 9);
    // echo '账号为：' . $zhanghao . '<br>';
    
    $mima = substr($str4, strpos($str4, '=', 9) + 1);
    // echo '密码为：' . $mima . '<br>';
    
    echo '<script>';
    echo 'login(' . $zhanghao . ',' . $mima . ');';
    echo '</script>';
} else {
    echo '<script>';
    echo "location.href='https://www.baidu.com';";
    echo '</script>';
    // echo '普通用户';
}

?>
```