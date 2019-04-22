## Go比较字符串时间值
Go支持通过一个模式layout来格式化时间或者解析时间。不过，注意下面这句话：**Layouts must use the reference time Mon Jan 2 15:04:05 MST 2006 to show the pattern with which to format/parse a given time/string.** 也就是说，格式模式必须是特定的时间点“Mon Jan 2 15:04:05 MST 2006”的不同格式，比如下面代码的"2006-01-02 15:04:05"。

### 第一步：把字符串时间解析为Go的Time类型
```go

func Parse(layout, value string) (Time, error)

```
time包的Parse把一个字符串格式的时间解析为一个Time类型的值，输入的字符串时间格式由layout参数指定。也就是说：**Parse根据layout解析value**。layout可以有多种格式，但是必须是特定的时间点：Mon Jan 2 15:04:05 MST 2006。

### 第二步：计算两个Time类型值的差
```go

func (t Time) Sub(u Time) Duration

```
Time类型的Sub方法返回一个Duration类型的值t-u。Duration类型表示两个时间点之间的时间差：
```go

type Duration int64
func (d Duration) Hours() float64
func (d Duration) Minutes() float64
func (d Duration) Seconds() float64
func (d Duration) Nanoseconds() int64
```
尽管Duration本质是一个int64类型，但是Go依然允许它有自己的方法。我们这里就用它的Seconds()方法计算这个时间差的值：
```go

updatedTimeStr := "2019-04-20 17:20:41"
format := "2006-01-02 15:04:05"
updatedTime, err := time.Parse(format, updatedTimeStr)
if err == nil {
    nowTime, err := time.Parse(format, time.Now().Format(format))
    if err == nil {
        if int64(nowTime.Sub(updatedTime).Seconds()) > 3600 {
            fmt.Println("data expired.")
        }
    }
}

```

