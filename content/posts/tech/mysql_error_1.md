---
title: "Mysql执行命令报:Ignoring query to other database"
date: 2021-01-04T13:10:20+08:00
lastmod: 2021-01-04T13:10:20+08:00
author: ["SuperEggs"]
tags:
  - mysql
---

今天登录服务器中，执行 mysql 命令：`show databases;`，结果报错了：
```shell
mysql> show databses;
Ignoring query to other database
```

检查半天才发现，原来是由于链接 mysql 命令有问题，

错误写法：
```shell
mysql -root -p
```

正确写法：
```shell
mysql -uroot -p
```

原来是少打了一个 `u`，退出去重新登录即可。

---

这里也有个疑问，为什么 `mysql -root -p`，也能登录成功呢？有知道的小伙伴可以在评论区留言，感谢了🙏
