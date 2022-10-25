---
title: "[Laravel 扩展包] 支持多主题：qirolab/laravel-themer"
date: 2022-10-25T22:55:05+08:00
lastmod: 2022-10-25T22:55:05+08:00
author: ["SuperEggs"]
tags:
- laravel
---



![](https://tva1.sinaimg.cn/large/008vxvgGly1h7hxfsw5lij31ao0dpmyk.jpg)

### 介绍

[qirolab/laravel-themer](https://github.com/qirolab/laravel-themer)扩展包为你 Laravel 应用程序提供多主题支持，它还为构建 Laravel 应用程序的起点提供了一个简单的身份验证脚手架。并且它还具有用于 Bootstrap，Tailwind，Vue 和 React 的预设。

**特点**

-   任意数量的主题

-   后备主题支持（WordPress 风格），它允许创建一个子主题来扩展任何主题

-   提供类似于 laravel/ui & laravel/breeze 的身份验证脚手架

-   导出所有 auth 控制器、测试和其他类似于 laravel/breeze 的文件

-   支持 Bootstrap、Tailwind、Vue 2、Vue 3 和 React

> Laravel Themer v2.x及以上版本支持 Vite。如果您想使用Laravel Mix，请尝试 Laravel Themer v1.7.1

### 安装

我的环境：

-   php ^8.0.2

-   laravel ^9.19

```bash
 composer require qirolab/laravel-themer
```

**发布配置文件**

 ```bash
 php artisan vendor:publish --provider="Qirolab\Theme\ThemeServiceProvider" --tag="config"
 ```bash

### 生成主题
```bash
➜ laravel (main) ✗ php artisan make:theme

 Name of your theme:
 > demo

 Select CSS Framework [Bootstrap]:
  [0] Bootstrap
  [1] Tailwind
  [2] Skip
 > 1

 Select Javascript Framework [Vue 3]:
  [0] Vue 3
  [1] React
  [2] Skip
 > 0

 Publish Auth Scaffolding [Views Only]:
  [0] Views Only
  [1] Controllers & Views
  [2] Skip
 > 0
 
Theme Name: demo
CSS Framework: Tailwind
JS Framework: Vue 3
Auth Scaffolding: Views Only

Theme scaffolding installed successfully.

Add following line in the `scripts` section of the `package.json` file:

"scripts": {
    ...

    "dev:demo": "vite --config themes/demo/vite.config.js",
    "build:demo": "vite build --config themes/demo/vite.config.js"
}

And please run `npm install && npm run dev:demo` to compile your fresh scaffolding.
 ```

### 通过中间件配置主题：

在 `app\Http\Kernel.php` 文件中注册 `ThemeMiddleware`:
```php
protected $routeMiddleware = [
  // ...
  'theme' => \Qirolab\Theme\Middleware\ThemeMiddleware::class,
 ];
```

使用中间件

```php
 // 示例1: 为路线设置主题
 Route::get('/dashboard', 'DashboardController@index')
  ->middleware('theme:dashboard-theme');
 
 
 // 示例2: 为路由组设置主题
 Route::group(['middleware'=>'theme:admin-theme'], function() {
  ...
 });
 
 
 // 示例3: 设置子主题和父主题
 Route::get('/dashboard', 'DashboardController@index')
  ->middleware('theme:child-theme,parent-theme');
```

### 常用命令

```php
use Qirolab\Theme\Theme;
 
 // 设置主题
 Theme::set('theme-name');
 
 // 获取当前主题
 Theme::active();
 
 // 获取当前父主题
 Theme::parent();
 
 // 关闭主题
 Theme::clear();
 
 // 获取当前主题路径
 Theme::path($path = 'views');
 // output:
 // /app-root-path/themes/active-theme/views
 
 // 获取指定主题的路径
 Theme::path($path = 'views', $themeName = 'admin');
 // output:
 // /app-root-path/themes/admin/views
 
 // 获取当前主题视图路径数组
 Theme::getViewPaths();
 // Output:
 // [
 //     '/app-root-path/themes/admin/views',
 //     '/app-root-path/resources/views' // 兜底视图路径
 // ]
```

### 运行 or 打包

```bash
npm run dev:demo
 
 # or
 
 npm run build:demo
```

### 总结

配置主题有三种方式：配置文件 → 路由中间件 → 内置方法

获取页面的顺序：子主题 → 父主题 → resources 文件夹

### 实践

在使用使用过程中，为了实现更复杂的功能，我封装了一个 Helper 类，大家可以参考使用：

```php
<?php

namespace App\Helpers;

use Illuminate\Support\Facades\Log;
use JsonException;
use Qirolab\Theme\Theme;

class ThemeHelper
{
    /**
     * 获取主题列表
     * @return array
     * @author super-eggs
     */
    public static function getThemeList(): array
    {
        $dir = config("theme.base_path");
        $dirs = array();
        if (!is_dir($dir)) {
            return $dirs;
        }

        $child_dirs = scandir($dir);
        foreach ($child_dirs as $child_dir) {
            if ($child_dir !== '.' && $child_dir !== '..') {

                $dirs[] = $child_dir;
            }
        }

        return $dirs;
    }

    /**
     * 获取主题配置列表
     * @return array
     * @author super-eggs
     */
    public static function getThemeInfoList(): array
    {
        $dirs = self::getThemeList();
        $configs = array();
        foreach ($dirs as $dir) {
            $full_dir = config("theme.base_path")."/".$dir;
            if (!is_file($full_dir.'/info.json')) {
                continue;
            }
            $json_string = file_get_contents($full_dir.'/info.json');
            try {
                $data = json_decode($json_string, true, 512, JSON_THROW_ON_ERROR);

            } catch (JsonException $e) {
                Log::error($e->getMessage());
                continue;
            }
            $data["is_set"] = false;
            if (Theme::path() === $full_dir) {
                $data["is_set"] = true;
            }
            $configs[] = $data;
        }

        return $configs;
    }

    /**
     * 获取指定主题下的页面列表(默认当前主题)
     * @author super-eggs
     */
    public static function getThemePageList(string $theme = null): array
    {
        if (!$theme) {
            $theme = Theme::active();
        }
        $themePath = Theme::viewPath($theme);

        return self::retrieveDir($theme, $themePath);
    }

    /**
     * 检索目录,查询页面
     *
     * @param  string  $theme  主题名称
     * @param  string  $themePath  主题视图目录
     * @return array
     * @author super-eggs
     */
    public static function retrieveDir(string $theme, string $themePath): array
    {
        $pages = [];
        if (!is_dir($themePath)) {
            return $pages;
        }
        $child_dirs = scandir($themePath);
        foreach ($child_dirs as $child_dir) {
            if ($child_dir === '.' || $child_dir === '..') {
                continue;
            }
            if (is_dir($themePath."/".$child_dir)) {
                $pages[(string) $child_dir] = self::retrieveDir($theme, $themePath."/".$child_dir);
            }
            if (str_ends_with($child_dir, ".blade.php")) {
                $pages[] = $child_dir;
            }

        }

        return $pages;
    }
}


```


在每个模板下面添加模板信息文件 `info.json`:
```json
{
    "theme": "demo",
    "title": "Demo 模板",
    "version": "1.0.0",
    "describe": "系统默认主题",
    "cover": "https://tva1.sinaimg.cn/large/e6c9d24ely1h1xpoqa91hj21ao0q974o.jpg",
    "tags": [
          "博客",
          "企业"
      ]
}
```
