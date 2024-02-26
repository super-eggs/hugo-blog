---
title: "Terraform 属性描述"
date: 2022-06-09T21:16:18+08:00
lastmod: 2022-06-09T21:16:18+08:00
author: ["SuperEggs"]
tags:
- Terraform
---

## ConflictsWith []string

是一组与此模式冲突的模式键。这只会检查它们是否在 _config _ 中设置。这不会为设置冲突键的故障资源引发错误。

一句话：不能同时配置！

## ExactlyOneOf []string

ExactlyOneOf是一组模式键，设置后，只能指定该列表中的一个键。如果也没有指定，它将出错。

一句话：必须设置一个。

## AtLeastOneOf []string

AtLeastOneOf是一组模式键，设置后，必须指定该列表中的至少一个键。

一句话：至少一个

## Deprecated string

如果设置了Deprecated，则表示不推荐使用此属性。
