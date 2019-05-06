## Go的类型断言
Go里面的类型断言只能应用于**接口类型**的值，形式如 x.(T)。类型断言用于判断一个接口值x的动态类型是否是T，即x是否一个特定类型T的值。

类型断言检查一个接口值x的动态类型是否是T，如果是（即检查成功），类型断言的结果就是x的动态值，结果的类型当然就是T。也就是说：类型断言就是把一个接口值的具体类型的值提取出来。

如果同时接收了类型断言的两个返回值，那么类型断言失败时不会崩溃，而是通过第二个返回值指明断言结果：失败（或者成功）。通常，类型断言可以写成如下紧凑的代码：
```go

if r, ok := e.(*Test); ok {
    // 这里可以使用*Test指针类型的值r
    ...
}

```

下面的是一个类型断言的例子：

```go

type IError interface {
	ErrCode() int
}

type AError struct {
	Err string
	Code int
}

func (a *AError)ErrCode() int {
	return a.Code
}

type BError struct {
	Err string
	Code int
}

func (a *BError)ErrCode() int {
	return a.Code
}

func test(i int) IError {
	if i % 2 ==0 {
		return &AError{Err : "ae",Code: 1}
	} else {
		return &BError{Err : "be",Code: 1}
	}
}

func main() {
	e := test(3)
	if _, ok := e.(*AError); ok {
		fmt.Println("e is AError.")
	}
	if _, ok := e.(*BError); ok {
		fmt.Println("e is BError.")
	}
}

// output: 
// > e is BError.

```

上面的示例表明，尽管AError 和 BError都是IError接口类型，并且它们的字段和方法完全一样，但是Go的类型断言依然能够区分开这两种类型。