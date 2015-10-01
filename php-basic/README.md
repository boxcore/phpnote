# PHP基础知识

[TOC]

一、PHP 标记
--------------
php文档的解析标识是`<?php 代码 ?>`，在`<?php`到`?>`之间的代码会被php解析器解析。

纯php文档建议不写php结束标签"?>",这可以避免在 PHP 结束标记之后万一意外加入了空格或者换行符，会导致 PHP 开始输出这些空白。

> 短标记 <? 和 ?>，开启方法有2种:

> 1. 修改php.ini 中的 `short_open_tag` 配置指令

> 2. 在编译 PHP 时使用了配置选项 `--enable-short-tags` 时才能使用短标记。



二、PHP注释
--------------

注释不会被php解析，添加注释是为了更好的描述代码的功能或特殊说明，php的注释风格如下：

```php
<?php
    echo "This is a test"; // 单行c++风格注释
    /* 多行注释风格
       yet another line of comment */
    echo "This is yet another test";
    echo 'One Final Test'; # 单行shell风格注释
?>
```
php的注释格式要求和方法请参考：<http://www.phpdoc.org>

