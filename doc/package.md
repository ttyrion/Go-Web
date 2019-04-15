## All about package in Go
Go的package声明一个Go包，包名对应一个目录名，包负责组织这个目录里的go代码文件。这一点类似Java的package。

### 导出变量
Go的包中，变量名首字母大写即可将其导出，其他名称的变量都是包私有的。这里的变量可以是任何类型，包括：常量、map、函数、结构体等等。

### 导入变量
import后面跟包名可以导入一个包。Go的import会根据包声明（包代码中的package声明）创建一个全局变量作为包的引用。
比如：
```go
/* src/Go-Web/calc.go */
package main

import "Go-Web/math"
import "fmt"

func main()  {
	fmt.Printf(fmt.Sprintf("%d", math.Add(1,2)));
}

/* src/Go-Web/math/add.go */
package math

func Add(a int, b int) int {
	return a + b;
}


```
**导入包时的查找路径：** 当导入一个包时，Go首先在**GOROOT/src**目录中查找这个包目录。如果没找到，就继续在**GOPATH/src**中查找该包目录。这也就是为什么上面的代码示例中，calc.go中导入math包时需要写"Go-Web/math"。

### 特殊的包：main
Go语言的每个代码文件都属于一个包。每个包都在一个单独的目录里面，不能把多个包放到同一个目录，也不能把同一个包的代码文件分拆到多个不同目录中。
也就是说：同一个目录下的所有go代码文件（不包含子目录代码文件）必须声明同一个包名。

一个标准的可执行Go程序必须有**package main**声明。
1. 如果一个Go程序声明了package main，那它就包含了一个main包。Go编译程序会把main包编译为二进制可执行文件(在**bin**目录中), 该可执行文件的入口是main包的main函数。程序编译时，会使用声明 main 包的代码所在的目录的目录名作为二进制可执行文件的文件名。
1. 如果一个Go程序还声明了除main之外的包，Go编译程序还会在**pkg**目录生成这些包的包归档文件（package archive file）。比如上面的示例，go install会在bin目录生成一个Go-Web.exe可执行文件，并且在pkg/Go-Web目录下面生成一个math.a包文件。

### 包的初始化
