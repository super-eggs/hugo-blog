---
title: "通过命令行将数据库数据导出"
date: 2021-07-19T20:21:49+08:00
lastmod: 2021-07-19T20:21:49+08:00
author: ["SuperEggs"]
tags:
  - mysql
---


如果你需要将数据库数据导出为文件, 你可以在 mysqldump 命令中指定数据库名及数据表。

在源主机上执行以下命令，将数据备份至 `backup.sql` 文件中:

```shell
$ mysqldump -uroot -p database_name table_name > /data/backup/backup.sql
password *****
```

如果完整备份数据库，则无需使用特定的表名称。

如果你需要将备份的数据库导入到MySQL服务器中，可以使用以下命令，使用以下命令你需要确认数据库已经创建：

```shell
$ mysql -uroot -p database_name < /data/backup/backup.sql
password *****
```