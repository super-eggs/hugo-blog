---
title: "理解 HTTP 幂等性"
date: 2023-07-04T22:19:45+08:00
lastmod: 2023-07-04T22:19:45+08:00
author: ["SuperEggs"]
tags:
  - HTTP
---

## 哪些叫幂等或/且安全的方法？

### 安全方法

安全方法是指不修改资源的 HTTP 方法。譬如，当使用 GET 或者 HEAD 作为资源 URL，都必须不去改变资源。然而，这并不全准确。意思是：它不改变资源的 表示形式。对于安全方法，它仍然可能改变服务器上的内容或资源，但这必须不导致不同的表现形式。

这表示下述是不对的，因为它实际上将删除博客文章：

```Bash
GET /blog/1234/delete HTTP/1.1
```

安全方法是那些可以被缓存、对资源无损预加载的方法。

### 幂等方法

HTTP 幂等方法是指无论调用多少次都不会有不同结果的 HTTP 方法。它无论是调用一次，还是十次都无关紧要。结果仍应相同。再次强调， 它只作用于结果而非资源本身。它仍可能被操纵（如一个更新的 timestamp），提供这一信息并不影响（当前）资源的表现形式。

考虑一下下面的结果：

`a = 4;`  不安全，但是幂等

`a++;`    不安全，也不幂等

第一个例子是幂等的：无论这个声明被执行多少次，a 总是等于 4。第二个例子并不是幂等的，执行 10 次将导致一个与执行 5 次所不一样的结果。由于两个例子 都改变 a 的值，因此两者都不是安全方法。

幂等对于构建一个容易（fault-tolerent）的 API 非常重要。假设一个客户端希望通过 POST 来更新一个资源，由于 POST 是一个非幂等方法，调用它多次 将导致错误的更新。那么当你向服务发送一个 POST 请求过程中超时将会发生什么，是不是资源已经被更新？超时发生在向服务器发送请求阶段还是服务返回 客户端阶段？我们是否能够安全地重试一遍，或第一要查出资源究竟发生了什么变化？通过幂等方法，我们则无须回答这种问题，直到我们从服务器得到返回， 我们可以安全地重发请求。

处理安全方法仍需谨慎：如果想要像 [GET] 这样看似安全的方法将更改资源，则您和服务器之间的任何中间件客户端代理系统都可能缓存此响应。另一个想要通过相同的url(如：http://example.org/api/article/1234/delete)，更改此资源的客户机不会调用服务器，而是直接从缓存返回信息。任何中间件代理都不会缓存非安全(和非幂等)方法。

### (部分) HTTP 方法概览

|HTTP Method|幂等|安全|
|-|-|-|
|OPTIONS|yes|yes|
|GET|yes|yes|
|HEAD|yes|yes|
|PUT|yes|no|
|POST|no|no|
|DELETE|yes|no|
|PATCH|no|no|




## HTTP的幂等性

HTTP协议本身是一种面向资源的应用层协议，但对HTTP协议的使用实际上存在着两种不同的方式：一种是RESTful的，它把HTTP当成应用层协议，比较忠实地遵守了HTTP协议的各种规定；另一种是SOA的，它并没有完全把HTTP当成应用层协议，而是把HTTP协议作为了传输层协议，然后在HTTP之上建立了自己的应用层协议。本文所讨论的HTTP幂等性主要针对RESTful风格的，不过正如上一节所看到的那样，幂等性并不属于特定的协议，它是分布式系统的一种特性；所以，不论是SOA还是RESTful的Web API设计都应该考虑幂等性。下面将介绍HTTP GET、DELETE、PUT、POST四种主要方法的语义和幂等性。

HTTP GET方法用于获取资源，不应有副作用，所以是幂等的。比如：`GET http://www.bank.com/account/123456` ，不会改变资源的状态，不论调用一次还是N次都没有副作用。请注意，这里强调的是一次和N次具有相同的副作用，而不是每次GET的结果相同。`GET http://www.news.com/latest-news` 这个HTTP请求可能会每次得到不同的结果，但它本身并没有产生任何副作用，因而是满足幂等性的。

HTTP DELETE方法用于删除资源，有副作用，但它应该满足幂等性。比如：`DELETE http://www.forum.com/article/4231` ，调用一次和N次对系统产生的副作用是相同的，即删掉id为4231的帖子；因此，调用者可以多次调用或刷新页面而不必担心引起错误。

比较容易混淆的是HTTP POST和PUT。POST和PUT的区别容易被简单地误认为“POST表示创建资源，PUT表示更新资源”；而实际上，二者均可用于创建资源，更为本质的差别是在幂等性方面。在HTTP规范中对POST和PUT是这样定义的：

> The POST method is used to request that the origin server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line ...... If a resource has been created on the origin server, the response SHOULD be 201 (Created) and contain an entity which describes the status of the request and refers to the new resource, and a Location header.

The PUT method requests that the enclosed entity be stored under the supplied Request-URI. If the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server. If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI.

POST所对应的URI并非创建的资源本身，而是资源的接收者。比如：`POST http://www.forum.com/articles` 的语义是在 `http://www.forum.com/articles` 下创建一篇帖子，HTTP响应中应包含帖子的创建状态以及帖子的URI。两次相同的POST请求会在服务器端创建两份资源，它们具有不同的URI；所以，POST方法不具备幂等性。而PUT所对应的URI是要创建或更新的资源本身。比如：`PUT http://www.forum/articles/4231` 的语义是创建或更新ID为4231的帖子。对同一URI进行多次PUT的副作用和一次PUT是相同的；因此，PUT方法具有幂等性。

在介绍了几种操作的语义和幂等性之后，我们来看看如何通过Web API的形式实现前面所提到的取款功能。很简单，用POST /tickets来实现create_ticket；用`PUT /accounts/account_id/ticket_id?amount=xxx` 来实现 idempotent_withdraw。值得注意的是严格来讲 amount 参数不应该作为URI的一部分，真正的URI应该是 `/accounts/account_id/ticket_id`，而amount应该放在请求的body中。这种模式可以应用于很多场合，比如：论坛网站中防止意外的重复发帖。
