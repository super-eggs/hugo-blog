---
title: "[Dcat 扩展] dcat-distpicker 省市区三级联动选择组件拓展"
date: 2022-06-13T22:52:03+08:00
lastmod: 2022-06-13T22:52:03+08:00
author: ["SuperEggs"]
tags:
- dcat-admin
- laravel
- php
---

[dcat-distpicker](https://github.com/super-eggs/dcat-distpicker) 是一个中国省市区三级联动选择组件，基于 `Distpicker` 实现的 `dcat-admin` 扩展，用来将 `Distpicker` 集成进 `dcat-admin`的表单中。
如果此插件给你带来的帮助，麻烦给我一个 `star`。当然如果你在使用过程中发现地区不完整的情况，欢迎随时反馈给我。

## 最近版本特色

- 修复一对多下的选择器显示异常 bug
- 新增支持多层级选择配置
- 更新 distpicker.js  到 2.0.7
- 新增支持地区名称回显
- 兼容 [laravel-wherehasin](https://github.com/jqhph/laravel-wherehasin)
- 新增支持筛选时选择框显示 tooltip

## 截图

![image-20200628150204971](https://cdn.learnku.com/uploads/images/202206/12/32195/38LTl0eK4z.jpeg!large)

## 安装

首先

```shell
# jqhph/dcat-admin 1.x
composer require "super-eggs/dcat-distpicker:^1.0"

# jqhph/dcat-admin 2.x
composer require "super-eggs/dcat-distpicker:^2.0"
```

然后: (dcat-admin 2.x 无需执行!!!)

```shell
php artisan admin:import dcat-distpicker
```

## 开启扩展

后台开启
- dcat-admin 2.x
![](https://cdn.learnku.com/uploads/images/202206/12/32195/I9wIgji02D.png!large)

## 使用

### 数据表单中使用

比如在表中有三个字段`province_id`, `city_id`, `district_id`, 在form表单中使用它：

```php
$form->distpicker(['province_id', 'city_id', 'district_id']);
```

设置默认值

```php
$form->distpicker([
    'province_id' => '省份',
    'city_id' => '市',
    'district_id' => '区'
], '地域选择')->default([
    'province' => 130000,
    'city'     => 130200,
    'district' => 130203,
]);
```

可以设置每个字段的placeholder

```php
// 省、市、区
$form->distpicker([
    'province_id' => '省',
    'city_id'     => '市',
    'district_id' => '区'
]);
// 省、市 (Available in v2.1.0+)
$form->distpicker([
    'province_id' => '省',
    'city_id'     => '市',
]);
// 只显示省 (Available in v2.1.0+)
$form->distpicker([
    'province_id' => '省',
]);
```

设置label

```php
$form->distpicker(['province_id', 'city_id', 'district_id'], '请选择区域');
```

设置自动选择, 可以设置1,2,3 表示自动选择到第几级

```php
$form->distpicker(['province_id', 'city_id', 'district_id'])->autoselect(1);
```

### 表格筛选中使用

```php
$filter->distpicker('province_id', 'city_id', 'district_id', '地域选择');
```

筛选同样支持多级选择:

```php
// 省、市 (Available in v2.1.0+)
$filter->distpicker('province_id', 'city_id','', '地域选择');
//or
$filter->distpicker('province_id', 'city_id');
// 只显示省 (Available in v2.1.0+)
$filter->distpicker('province_id','','', '地域选择');
//or
$filter->distpicker('province_id');
```

### 数据表格中使用

省市区名称回显 (Available in v2.1.0+):

```php
$grid->column('province_id')->distpicker();
$grid->column('city_id')->distpicker();
$grid->column('district_id')->distpicker();
```

并且提供了一个全局可用的辅助函数:

```php
use SuperEggs\DcatDistpicker\DcatDistpickerHelper;

DcatDistpickerHelper::getAreaName($code); // return string
```

## 地区编码数据

[Distpicker](https://github.com/fengyuanchen/distpicker) 所使用的地域编码是基于国家统计局发布的数据, 数据字典为`china_area.json`文件.

## 鸣谢

由衷感谢以下开源软件、框架等（包括但不限于）

- [laravel](https://laravel.com)
- [jqhph/dcat-admin](https://github.com/jqhph/dcat-admin)
- [fengyuanchen/distpicker](https://github.com/fengyuanchen/distpicker)
- [laravel-admin-extensions/china-distpicker](https://github.com/laravel-admin-extensions/china-distpicker)

