> Go语言

* [基础语法](语言/Go/basic.md)

[TOC]


### 一、基础语法

#### 1、变量

#### 2、程序结构

* **循环**

for循环的三种方式：<br/>
* 基于计数器的迭代
  基本形式为：`for 初始化语句; 条件语句; 修饰语句 {}`，例如：
```go
for i := 0; i < 5; i++ {
		fmt.Printf("This is the %d iteration\n", i)
	}
```

* 基于条件的判断的迭代<br/>
  类似于其他语言中的while语句，基本形式为：`for 条件语句 {}`。例如：

```go
package main
import "fmt"

func main() {
	var i int = 5

	for i >= 0 {
		i = i - 1
		fmt.Printf("The variable i is now: %d\n", i)
	}
}
```

* for-range<br/>
  类似于其他语言中的foreach语句，基本格式为：`for ix, val := range coll { }`,例如：

  ```go
  package main

  import "fmt"

  func main() {
    //迭代字符串
  	str := "Go is a beautiful language!"
  	fmt.Printf("The length of str is: %d\n", len(str))
  	for pos, char := range str {
  		fmt.Printf("Character on position %d is: %c \n", pos, char)
  	}
    //
  }
  ```
要注意的是，val 始终为集合中对应索引的值拷贝，因此它一般只具有只读性质，对它所做的任何修改都不会影响到集合中原有的值（译者注：如果 val 为指针，则会产生指针的拷贝，依旧可以修改集合中的原值）。一个字符串是 Unicode 编码的字符（或称之为 rune）集合，因此您也可以用它迭代字符串。
