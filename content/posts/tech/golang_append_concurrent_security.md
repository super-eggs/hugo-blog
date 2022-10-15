---
title: "append() 并发安全问题"
date: 2022-06-16T23:01:48+08:00
lastmod: 2022-06-16T23:01:48+08:00
author: ["SuperEggs"]
tags: 
- golang
---

## 由来：

今天在编程过程中遇到这样一个问题，并发把数据写入到切片中。

```go
var posts []int

func hello(i int) {
	defer wg.Done() // goroutine结束就登记-1
	posts = append(posts, i)
}
func main() {
	for i := 0; i < 10; i++ {
		wg.Add(1) // 启动一个goroutine就登记+1
		go hello(i)
	}
	wg.Wait() // 等待所有登记的goroutine都结束
	fmt.Println(posts)
}
```

结果输出各不相同：

~~~go
//输出
➜ go run main.go
[5 4 6 1 0 2 9 7 8]
➜ go run main.go
[0 9 4]
➜ go run main.go
[0 3 1 2 5 4 6 7 9 8]
➜ go run main.go
[0 8 7 9]
~~~

## 原因：

当A和B两个协程运行append的时候同时发现posts[1]这个位置是空的，他们就都会把自己的值放在这个位置，这样他们两个的值就会覆盖，造成数据丢失。

## 解决：

最简单的方式就是用锁，贴一个例子

~~~go
var posts []int
var mutex sync.Mutex
var wg    sync.WaitGroup

func hello(i int) {
	defer wg.Done() // goroutine结束就登记-1
	mutex.Lock()
	posts = append(posts, i)
	mutex.Unlock()
}
func main() {
	for i := 0; i < 10; i++ {
		wg.Add(1) // 启动一个goroutine就登记+1
		go hello(i)
	}
	wg.Wait() // 等待所有登记的goroutine都结束
	fmt.Println(posts)
}
~~~



