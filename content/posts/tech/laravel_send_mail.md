---
title: "Laravel 使用 163邮箱发送邮件"
date: 2021-03-17T09:03:51+08:00
lastmod: 2021-03-17T09:03:51+08:00
author: ["SuperEggs"]
tags: 
- laravel
---


## 安装教程

### 环境介绍

- laravel：6.8
- PHP：7.3
- 服务器：Ubuntu
- ECS服务商：阿里云
- 邮件服务商：163邮箱

### 开启 163 邮箱的 SMTP 支持

![image-20200716144130662](https://tva1.sinaimg.cn/large/e6c9d24ely1h3aho3zjrfj218e0cywfs.jpg)

发送短信验证身份后，会得到一个『授权码』，复制方框里的『授权码，授权码将作为我们的密码使用。

### 163邮箱端口

![image-20200716144421837](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahnve5z6j20v409wmy6.jpg)

### Laravel 邮箱发送配置

Laravel 中邮箱发送的配置存放于 `config/mail.php` 中。不过 `mail.php` 中我们所需的配置，都可以通过 `.env` 来配置。作为最佳实践，我们优先选择通过环境变量来配置：

*.env*

```php
.
.
.
MAIL_DRIVER=smtp
MAIL_HOST=smtp.163.com
MAIL_PORT=465
MAIL_USERNAME=xxxxxxxxxxxxxx@163.com
MAIL_PASSWORD=xxxxxxxxx
MAIL_ENCRYPTION=ssl
MAIL_FROM_ADDRESS=xxxxxxxxxxxxxx@163.com
MAIL_FROM_NAME=Laravel
.
.
.
```

选项讲解：

1. MAIL_DRIVER=smtp —— 使用支持 ESMTP 的 SMTP 服务器发送邮件；
2. MAIL_HOST=smtp.163.com —— 163 邮箱的 SMTP 服务器地址，必须为此值；
3. MAIL_PORT=465 —— 163 邮箱的 SMTP 服务器端口，这里需要注意，如果使用 tls 加密方式，端口设置为：25，如果有ssl 证书，那我们使用 ssl 加密，端口设置为：465/994 。阿里云的 25 端口解封非常困难，所以这里我们使用 ssl 加密，端口使用 465 。
4. MAIL_USERNAME=xxxxxxxxxxxxxx@163.com —— 请将此值换为你的 163邮箱地址
5. MAIL_PASSWORD=xxxxxxxxx —— 密码是我们上边拿到的授权码；
6. MAIL_ENCRYPTION=ssl —— 加密类型，选项 null 表示不使用任何加密，这里还可以选择ssl、tls，因为上边提过，大多数服务商把服务器的 25 端口统统封了，所以我们这里使用 ssl 加密，和上边呼应。
7. MAIL_FROM_ADDRESS=xxxxxxxxxxxxxx@qq.com —— 此值必须同 MAIL_USERNAME 一致，也就是你的 163邮箱；
8. MAIL_FROM_NAME=Laravel —— 用来作为邮件的发送者名称。



### 阿里云开启 465 端口

![image-20200716145039393](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahofqjkoj21t208kjs8.jpg)



### 最后测试一下

~~~php
public function send()
{
  \Mail::raw('这是一封测试邮件', function ($message) {
    $to = 'XXXX@qq.com';
    $message ->to($to)->subject('测试邮件');
  });
}

$this->send();
    
~~~



## 报错处理

> 错误信息：Expected response code 220 but got an empty response

![image-20200716145434652](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahpe3idkj21mw0p6ae9.jpg)

这个报错，是因为 `MAIL_ENCRYPTION` 的值有问题。



> Connection could not be established with host smtp.163.com :stream_socket_client(): unable to connect to ssl://smtp.163.com:25 (Connection timed out)

![image-20200716145751235](https://tva1.sinaimg.cn/large/e6c9d24ely1h3ahoo67o5j21q80kyn2j.jpg)

链接超时，25 端口没有开放。可以尝试使用其他端口

