# Go
[Go Workspace](https://github.com/ttyrion/Go-Web/blob/master/doc/workspace.md)

[All about package in Go](https://github.com/ttyrion/Go-Web/blob/master/doc/package.md)

[命令行参数解析](https://github.com/ttyrion/Go-Web/blob/master/doc/flag.md)

[Go字符串时间比较](https://github.com/ttyrion/Go-Web/blob/master/doc/time.md)

[类型断言](https://github.com/ttyrion/Go-Web/blob/master/doc/type-assertions.md)


# 参考
[黄健宏 - 主页）](http://huangz.me/)

### 杂记
#### VSCode + godef 浏览Go代码
[godef](https://github.com/rogpeppe/godef) 是一个支持在浏览Go代码时可调转到变量、函数的定义的项目。在VSCode中按快捷键F12即可跳转到变量或函数的定义处。如果还没有安装godef，VSCode会提示安装。也可以手动下载godef项目文件到src/github.com/rogpeppe/godef目录下面。

另外，为方便在代码之间来回跳转，可以自定义VSCode的**Go Back/Forward**快捷键。


## VSCode调试GO
配置launch.json，添加调试项可以根据需要以各种参数启动调试，比如：
```javascript

{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Current",
      "type": "go",
      "request": "launch",
      "mode": "debug",
      "program": "${file}"
    },
    {
      "name": "GOP Launch",
      "type": "go",
      "request": "launch",
      "mode": "auto",
      "program": "${workspaceFolder}\\src\\GOP\\main.go",
      "env": {},
      "args": []
    },
    {
      "name": "GOP Sort",
      "type": "go",
      "request": "launch",
      "mode": "auto",
      "program": "${workspaceFolder}\\src\\GOP\\main.go",
      "env": {},
      "args": ["--mode=sort", "--order=reverse"]
    }
  ]
}

```
根据上面的配置，调试项目的时候就可以很灵活，比如可以以特定命令行参数启动项目，或者直接调试当前打开的文件（文件里面有main函数）：只需要在DEBUG列表中选择一个调试项作为默认调试项就行。