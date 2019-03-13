> Go语言

### 一、基础语法

#### 1、变量
  变量的命名方式与Java一致，有效的标识符必须以字母（可以使用任何 UTF-8 编码的字符或 \_）开头，然后紧跟着 0 个或多个字符或 Unicode 数字，如：X56、group1、\_x23、i、өԑ12。

   Go 代码中会使用到的 25 个关键字或保留字：

  | break| default |func 	|interface |	select|
  | :--- | :------ |:--- | :------ |:--- |
  |case 	|defer 	|go 	|map 	|struct|
  |chan 	|else 	|goto 	|package 	|switch|
  |const 	|fallthrough 	|if 	|range 	|type|
  |continue 	|for 	|import 	|return 	|var|

除了以上介绍的这些关键字，Go 语言还有 36 个预定义标识符，其中包含了基本类型的名称和一些基本的内置函数（第 6.5 节），它们的作用都将在接下来的章节中进行进一步地讲解。

|append 	|bool 	|byte 	|cap 	|close 	|complex 	|complex64 	|complex128 	|uint16|
| :--- | :------ |:--- | :------ |:--- |:------ |:--- | :------ |:--- |
|copy 	|false 	|float32 	|float64 	|imag 	|int 	|int8 	|int16 	|uint32
|int32 	|int64 	|iota 	|len 	|make 	|new 	|nil 	|panic 	|uint64
|print 	|println 	|real 	|recover 	|string 	|true 	|uint 	|uint8 	|uintptr

程序一般由关键字、常量、变量、运算符、类型和函数组成。

程序中可能会使用到这些分隔符：括号 ()，中括号 [] 和大括号 {}。

程序中可能会使用到这些标点符号：.、,、;、: 和 …。

程序的代码通过语句来实现结构化。每个语句不需要像 C 家族中的其它语言一样以分号 ; 结尾，因为这些工作都将由 Go 编译器自动完成。

如果你打算将多个语句写在同一行，它们则必须使用 ; 人为区分，但在实际开发中我们并不鼓励这种做法。

#### 2、程序的基本结构

* **包**

你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。

一个应用程序可以包含不同的包，而且即使你只使用 main 包也不必把所有的代码都写在一个巨大的文件里：你可以用一些较小的文件，并且在每个文件非注释的第一行都使用 package main 来指明这些文件都属于 main 包。如果你打算编译包名不是为 main 的源文件，如 pack1，编译后产生的对象文件将会是 pack1.a 而不是可执行程序。另外要注意的是，所有的包名都应该使用小写字母。

``` go
//引入包
import "fmt"
import "os"

import "fmt"; import "os"

import (
   "fmt"
   "os"
)

import ("fmt"; "os")
```

当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 private ）。

* **函数**

这是定义一个函数最简单的格式：

``` go
//你可以在括号 () 中写入 0 个或多个函数的参数（使用逗号 , 分隔），每个参数的名称后面必须紧跟着该参数的类型。
func functionName()
```

函数里的代码（函数体）使用大括号 {} 括起来。

左大括号 { 必须与方法的声明放在同一行，这是编译器的强制规定，否则你在使用 gofmt 时就会出现错误提示,  右大括号 } 需要被放在紧接着函数体的下一行。如果你的函数非常简短，你也可以将它们放在同一行：
``` go
func Sum(a, b int) int { return a + b }
```
对于大括号 {} 的使用规则在任何时候都是相同的（如：if 语句等）。

因此符合规范的函数一般写成如下的形式：
``` go
func functionName(parameter_list) (return_value_list) {
   …
}
```
其中：

* parameter_list 的形式为 (param1 type1, param2 type2, …)
* return_value_list 的形式为 (ret1 type1, ret2 type2, …)

只有当某个函数需要被外部包调用的时候才使用大写字母开头，并遵循 Pascal 命名法；否则就遵循骆驼命名法，即第一个单词的首字母小写，其余单词的首字母大写。

下面这一行调用了 fmt 包中的 Println 函数，可以将字符串输出到控制台，并在最后自动增加换行字符 \n：

``` go
fmt.Println("hello, world")
```
使用 `fmt.Print("hello, world\n")`` 可以得到相同的结果。

返回列表放在参数列表后面

* **类型**

使用 var 声明的变量的值会自动初始化为该类型的零值。类型定义了某个变量的值的集合与可对其进行操作的集合。

类型可以是基本类型，如：int、float、bool、string；结构化的（复合的），如：struct、array、slice、map、channel；只描述类型的行为的，如：interface。

函数也可以是一个确定的类型，就是以函数作为返回类型。这种类型的声明要写在函数名和可选的参数列表之后，例如：
``` go
func FunctionName (a typea, b typeb) typeFunc
```

Go程序的一般结构

``` go
package main

import (
   "fmt"
)

const c = "C"

var v int = 5

type T struct{}

func init() { // initialization of package
}

func main() {
   var a int
   Func1()
   // ...
   fmt.Println(a)
}

func (t T) Method1() {
   //...
}

func Func1() { // exported function Func1
   //...
}
```

* 在完成包的 import 之后，开始对常量、变量和类型的定义或声明。
* 如果存在 init 函数的话，则对该函数进行定义（这是一个特殊的函数，每个含有该函数的包都会首先执行这个函数）。
* 如果当前包是 main 包，则定义 main 函数。
*然后定义其余的函数，首先是类型的方法，接着是按照 main 函数中先后调用的顺序来定义相关函数，如果有很多函数，则可以按照字母顺序来进行排序。

* **类型的转换**

在必要以及可行的情况下，一个类型的值可以被转换成另一种类型的值。由于 Go 语言不存在隐式类型转换，因此所有的转换都必须显式说明，就像调用一个函数一样（类型在这里的作用可以看作是一种函数）：

``` go
valueOfTypeB = typeB(valueOfTypeA)
```

**类型 B 的值 = 类型 B(类型 A 的值)**

示例：
``` go
a := 5.0
b := int(a)
```

* **常量**
常量使用关键字 const 定义，用于存储不会改变的数据。

存储在常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

常量的定义格式：·const identifier [type] = value·，例如：
``` go
const Pi = 3.14159
```
