---
title: "[Terraform] 对资源名称使用多个循环（循环拼接两组变量）"
date: 2022-07-13T23:29:06+08:00
lastmod: 2022-07-13T23:29:06+08:00
author: ["SuperEggs"]
tags: 
- terraform
---


## 案例一：

通过循环两个`list(string)`类型的变量来动态创建AWS S3 bucket的方法。这就是我目前的情况：

```go
variable "network" {
  type    = list(string)
  default = ["blue", "green", "orange"]
}

variable "type" {
  type    = list(string)
  default = ["ClientA", "ClientB"]
}

resource "aws_s3_bucket" "this" {
  for_each = toset([for p in setproduct(var.type, var.network) : "${p[0]}-${p[1]}"])
  bucket   = "${each.key}.mysite.com"
  tags     = local.tags
}

```

输出：

```go
ClientA-blue.mysite.com
ClientA-green.mysite.com
ClientA-orange.mysite.com
ClientB-blue.mysite.com
ClientB-green.mysite.com
ClientB-orange.mysite.com
```

## 案例二：

```go
variable "list1" {
  default = ["a","b","c","d"]
}

variable "list2" {
  default = ["1","2","3","4"]
}

locals {
  list3 = [for p in setproduct(var.list1, var.list2) : "${p[0]}${p[1]}"]
}

output "list3" {
  value = local.list3
}
```

输出：

```go
Changes to Outputs:
  + list3 = [
      + "a1",
      + "a2",
      + "a3",
      + "a4",
      + "b1",
      + "b2",
      + "b3",
      + "b4",
      + "c1",
      + "c2",
      + "c3",
      + "c4",
      + "d1",
      + "d2",
      + "d3",
      + "d4",
    ]
```

