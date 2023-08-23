---
title: golang模板语法及预定义函数
date: 2023-08-23 14:52:39
categories:
  - [golang]
tags: golang
---
在Go语言中，使用模板可以通过`text/template`或者`html/template`包来实现。这两个包的主要区别是，`html/template`包会自动对输出进行转义，以防止跨站点脚本攻击。

下面是`text/template`和`html/template`包中常用的模板语法和示例：

1. 注释：

   可以使用`{{/* 注释内容 */}}`语法添加注释。注释可以在模板执行时忽略。

   示例：
   ```
   {{/* This is a comment */}}
   ```

2. 输出变量值：

   使用`{{.}}`语法输出当前的上下文变量值。

   示例：
   ```
   {{.}}  // 输出当前上下文的变量值
   ```

3. 条件语句：

   使用`{{if .Condition}} ... {{else}} ... {{end}}`语法进行条件判断。

   示例：
   ```
   {{if .Condition}}
       True Block
   {{else}}
       False Block
   {{end}}
   ```

4. 循环语句：

   使用`{{range .Slice}} ... {{end}}`语法进行循环迭代。

   示例：
   ```
   {{range .Slice}}
       {{.}}  // .表示当前迭代的元素
   {{end}}
   ```

5. 定义和使用变量：

   使用`{{with .Variable}} ... {{end}}`语法定义和使用临时变量。

   示例：
   ```
   {{with .Variable}}
       {{.}}  // 使用临时变量
   {{end}}
   ```

6. 函数调用：

   使用`{{函数名 参数1 参数2 ...}}`语法调用内置或自定义函数。

   示例：
   ```
   {{len .String}}  // 调用len函数返回字符串的长度
   ```

7. 嵌套模板：

   使用`{{template "模板名称" .数据}}`语法在模板中嵌套另一个模板。

   示例：
   ```
   {{template "header" .PageTitle}}  // 嵌套名为"header"的模板，并传入.PageTitle参数
   ```

这些是常用的模板语法和示例，通过这些语法可以实现动态生成文本或者HTML代码的功能。具体使用方法可以参考Go语言文档中的模板包说明：https://golang.org/pkg/html/template/

在Go语言的模板中，可以通过在模板中调用预定义的全局函数来进行一些常用的操作。Go语言的模板引擎提供了一些内置函数，这些函数可以在模板中直接使用，而不需要额外的导入。

以下是一些常用的预定义全局函数：

1. `and`：接受任意数量的布尔值作为参数，并返回它们的与运算结果。
示例：
```
{{and true true}}  // true
{{and true false}} // false
```

2. `or`：接受任意数量的布尔值作为参数，并返回它们的或运算结果。
示例：
```
{{or true true}}  // true
{{or true false}} // true
```

3. `not`：接受布尔值作为参数，并返回它的否定值。
示例：
```
{{not true}}  // false
{{not false}} // true
```

4. `eq`：用于比较两个值是否相等。
示例：
```
{{eq 10 10}}   // true
{{eq "abc" "def"}} // false
```

5. `ne`：用于比较两个值是否不相等。
示例：
```
{{ne 10 20}}   // true
{{ne "abc" "abc"}} // false
```

6. `lt`：用于比较两个值是否左边小于右边。
示例：
```
{{lt 10 20}}   // true
{{lt "abc" "def"}} // true
```

7. `le`：用于比较两个值是否左边小于等于右边。
示例：
```
{{le 20 20}}   // true
{{le "abc" "def"}} // true
```

8. `gt`：用于比较两个值是否左边大于右边。
示例：
```
{{gt 20 10}}   // true
{{gt "def" "abc"}} // true
```

9. `ge`：用于比较两个值是否左边大于等于右边。
示例：
```
{{ge 20 20}}   // true
{{ge "def" "abc"}} // true
```

10. `len`：返回一个字符串或数组的长度。
示例：
```
{{len "hello"}}  // 5
{{len .slice}}   // 数组或切片的长度
```

以上就是一些常用的预定义全局函数的使用方法和示例。使用这些函数可以在模板中进行一些基本的逻辑操作和比较。