# Go语法

---

## 声明变量

```go
var message string
message = fmt.Sprintf("Hi, %v. Welcome!", name)

// 或
var message string = fmt.Sprintf("Hi, %v. Welcome!", name)
```

或者简写成：

```go
// go可以根据等式右侧的值，来判断变量的类型
message := fmt.Sprintf("Hi, %v. Welcome!", name)
```



## 定义函数

### 普通函数

```go
func Hello(name string) string {
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```

`func Hello(name string) string`

- `func` : 定义函数的关键字
- `Hello` : 函数名（在一个包内，大写字母开头的成员，外部可以访问；小写字母开头的成员，外部不可以访问）
- `name`  : 参数名称
- 第一个 `string` : 参数类型
- 第二个 `string` : 返回值类型

**注意：**

1. 在同一模块的同一个包（一个包包含多个 go 文件）中，不能声明 **同名的函数** ，否则调用时会 **报错** 。

### 返回多个值

```go
func Hello(name string) (string, int) {
    // do something
}
```

- 在Go中，一个函数可以返回多个值。

## Module

- 一个 `Module` 包含多个 `Package` ，一个 `Package` 包含多个 `Go` 源文件。

### import

```go
import (
    "fmt"
    "example.com/greetings"
)
```

- 导入多个包使用 `()` 包裹。

### go mod edit

当你要使用一个没有发布的 `本地模块` （比如 greetings ） 时，你需要在 **使用 greetings 的模块中** ，使用此命令来指定本地模块的位置：

```go
go mod edit -replace example.com/greetings=../greetings
```

`go.mod` 文件将新增如下内容：

```go
replace example.com/greetings => ../greetings
```

在再  **使用 greetings 的模块中**  执行  `go mod tidy` ，以同步 `go.mod` 文件：

```go
E:\codes\go\learn\hello>go mod tidy
go: found example.com/greetings in example.com/greetings v0.0.0-00010101000000-000000000000
```

## 错误处理

### 基础使用

#### 被调用方（返回异常类型）

```go
package greetings

import (
    "errors"
    "fmt"
)

// Hello returns a greeting for the named person.
func Hello(name string) (string, error) {
    // If no name was given, return an error with a message.
    if name == "" {
        return "", errors.New("empty name")
    }

    // If a name was received, return a value that embeds the name
    // in a greeting message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message, nil
}
```

- 导入 `errors` 包。
- 返回值增加一个 `error` 类型。
- 有错误时，调用 `errors.New()` 方法，返回一个 `error` 。
- 无错误时，返回 `nil` ，即 **空** 。

#### 调用方（判断是否异常）

```go
package main

import (
    "fmt"
    "log"

    "example.com/greetings"
)

func main() {
    // Set properties of the predefined Logger, including
    // the log entry prefix and a flag to disable printing
    // the time, source file, and line number.
    log.SetPrefix("greetings: ")
    log.SetFlags(0)

    // Request a greeting message.
    message, err := greetings.Hello("")
    // If an error was returned, print it to the console and
    // exit the program.
    if err != nil {
        log.Fatal(err)
    }

    // If no error was returned, print the returned message
    // to the console.
    fmt.Println(message)
}
```

- `log` 包的相关内容参考：https://pkg.go.dev/log