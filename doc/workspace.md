## Go Workspace
Go程序可以被放在系统的任意目录，通过 **go run hello.go**就能执行当前目录下的hello.go。

但是Go还有一个**workspace**的概念，可以帮助我们更好地管理Go项目。一个workspace，可以简单地认为就是一个系统目录，Go在此目录中查找源代码、管理依赖的包以及生成可发布的二进制文件。Go的import语句会在Go的标准库目录（**$GOROOT/src**）中查找包。如果这里没找到，Go会在 **$GOPATH/src** 目录中继续查找。其中**GOPATH**是一个系统变量，其值就是Go的**workspace**目录。

也就是说：**Go导入一个包时，先在标准库目录查找包，接着再在workspace目录中查找**。

### Workspace 目录结构
一个go workspace目录的结构应该如下：
```Go

GoWorkspaceDir
├── bin
├── pkg
└── src
     ├── project1
     └── project2

```
开始我们只需要创建src目录，build或install时，Go自会创建出bin和pkg目录。
