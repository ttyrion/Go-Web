### 命令行参数解析
Go的flag包提供了一系列解析命令行参数的接口。

#### 命令行标签语法（Command line flag syntax）
Go允许以下三种命令行格式：
```javascript

-flag    // 只支持bool类型
-flag=x
-flag x  // 只支持非bool类型

```
参数flag前一个短横线或者两个短横线都可以。

#### 解析命令行参数的方法
##### 第一步： 定义flag变量
flag变量会在解析结束持有命令行参数的值。有三种方式定义它们：
**方式一：** 通过flag.**String()**, **Bool()**, **Int()** 等方法，这种方式返回一个指针类型。如：
```javascript

import "flag"

// 参数：命令行参数flag名称，默认值，使用说明
var port = flag.Int("port", 8080, "port should be listened.")

```

**方式二：** 通过flag.**XVar()**方法将flag绑定到一个对应类型的变量，这些接口没有返回值，如
```javascript

import "flag"

var port int

func init() {
    flag.IntVar(&port, "port", 8080, "port should be listened.")
}

```

**方式三：** 通过flag.**Var()**方法将flag绑定到一个自定义类型的变量，自定义类型**必须实现flag.Value接口**并且**接收者必须是指针类型**。
flag.Value接口类型定义如下：
```javascript

type Value interface {
        String() string
        Set(string) error
}

```

##### 第二步：执行期解析命令行参数
这一步只需调用flag.Parse()即可。

##### 第三步：这里就可以访问flag变量的值了
flag.Parse()解析完成后，我们就可以访问第一步定义的flag变量（可称之为命令行参数变量）其类型可以是指针类型或者自定义类型。


完整的一个代码示例如下：
```javascript

package main

import _ "Go-Web/math"
import "fmt"
import "flag"
import "strings"
import "errors"

type Student struct {
	Name string;
	Id string
}

func (s Student) String() string {
	return s.Name + "&" + s.Id;
}

func (s *Student) Set(cmdline string) error {
	inputs := strings.Split(cmdline, ":")
	if len(inputs) != 2 {
		return errors.New("invalid value for -student.")
	} else {
		s.Name = inputs[0]
		s.Id = inputs[1]
	}

	return nil
}

var student Student

func init() {
	flag.Var(&student, "student", "specify a student")
}

func main()  {
	flag.Parse();
	fmt.Printf("Student %s is specified.", student);
}

```