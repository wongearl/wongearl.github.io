---
title: go mod使用最新提交
date: 2023-08-18 18:49:14
tags: golang
---
​
例如一个项目在其中依赖了    github.com/linuxsuren/go-fake-runtime v0.0.1

go.mod内容：

	github.com/linuxsuren/go-fake-runtime v0.0.1

修改了github.com/linuxsuren/go-fake-runtime代码，存在一个最新的commit hash值为25fa814c6232e545f5bce03bd4db04fc37e10250

修改项目中的go.mod
```
github.com/linuxsuren/go-fake-runtime 25fa814c6232e545f5bce03bd4db04fc37e10250
```
然后执行go mod tidy,会看到go.mod中的依赖会更新为最新的提交
```
github.com/linuxsuren/go-fake-runtime v0.0.2-0.20230815071200-25fa814c6232
```
至此项目依赖的github.com/linuxsuren/go-fake-runtime已由v0.0.1版本更为指定的commit。

​