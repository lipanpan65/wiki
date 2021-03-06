# String类型

字符串是一个不可改变的字节序列，Go中的String通常是用来包含人类可读的文本，文本中字符串通常被解释为UTF8编码的Unicode码点，Go的字符串是由单个字节连接起来的

## 字符串的本质



示例：字符串的本质 utf-8 编码的字节序列
```go
// unicode 字符集：文字 -> 码点（ucs4 4个字节表示）
//                          |
//                        utf-8 : 对 码点 进行编码 -> 10101010 
var name string = "李盼盼"
// 获取的 name[0] 表示的是字节 230 默认为十进制
fmt.Println(name[0]) // 230
fmt.Println(name[1]) // 157
fmt.Println(name[2]) // 142
// 将获取到的十进制 转换为 二进制 计算机执行的二进制文件
fmt.Println(name[0], strconv.FormatInt(int64(name[0]), 2)) // 11100110
fmt.Println(name[1], strconv.FormatInt(int64(name[1]), 2)) // 10011101
fmt.Println(name[2], strconv.FormatInt(int64(name[2]), 2)) // 10001110
// 字符串的长度 9 : 表示的是 utf-8编码的字节长度
fmt.Println(len(name))
```
## 基本操作



### 字符串的长度

```go

```

### 是否已xx开头 `HasPrefix`

```go
var baidu string = "www.baidu.com"
google := "www.google.com"
fmt.Println(baidu, google)
// 是否 以 www 开头
fmt.Println(strings.HasPrefix(baidu,"www")) // true
```

### 是否以xx结尾 `HasSuffix`

```go
fmt.Println(strings.HasSuffix(baidu,"com"))
```

### 是否包含 `Contains`

```go
fmt.Println(strings.Contains(google,"google")) // true
```

### 变大写 `ToUpper`

```go
fmt.Println(strings.ToUpper(baidu)) // WWW.BAIDU.COM
```

### 变小写 `ToLower`

```go
job := "DBA"
fmt.Println(strings.ToLower(job)) // dba
```

### 去两边 ``

```go

```

### 首字母大写 `Title`

```go
title := "hello"
fmt.Println(strings.Title(title)) // Hello
```

### 替换

```go

```

### 分割

```go

```

### 拼接

```go

```

### 字符串格式化 `fmt.Sprintf`
```
// 字符串格式化
var who, address, action string

fmt.Println("请输入姓名")
fmt.Scanln(&who)

fmt.Println("请输入地址")
fmt.Scanln(&address)

fmt.Println("请输入你在干什么")
fmt.Scanln(&action)

result := fmt.Sprintf("我叫%s,在%s正在干%s", who, address, action)
fmt.Println(result)
```




示例：字符串不可改变

```go
text := "this is string"
text[0] = "that" // ./main.go:24:10: cannot assign to text[0] (strings are immutable)
fmt.Println(text[0])
```
示例：字符串的转义字符
+ 字符串支持常见的转义字符 
+ 反引号包裹的字符串会原样输出，即使存在空格，换行符号等



```go
text := "this is string"
//text[0] = "that" // ./main.go:24:10: cannot assign to text[0] (strings are immutable)
fmt.Println(text[0])
text3 := "info \n T"
fmt.Print(text3)
```
示例：字符串的拼接
```go

```
示例：字符串的索引
```go

```
示例：字符串的遍历
```go

```
示例：字符串转换为字节集合
```go
// 字节集合
byteSet := []byte(name)
fmt.Println(byteSet) // [230 157 142 231 155 188 231 155 188] 十进制展示 存贮二进制
// 字节集合转换为字符串
byteList := []byte{230, 157, 142, 231, 155, 188, 231, 155, 188}
targetString := string(byteList)
fmt.Println(targetString)
```
示例：将字符串转换为 unicode 字符集码点的集合 展示是默认按照 十进制 展示
```go
// 将 unicode 转换为字符集码点的集合
tempSet := []rune(name)
fmt.Println(tempSet) // [26446 30460 30460] 默认按照十进制展示
fmt.Println(tempSet[0], strconv.FormatInt(int64(tempSet[0]), 16)) // 26446 674e
// 将 rune 集合 转换字符串
runeList := []rune{26446,30460,30460}
targetName := string(runeList)
fmt.Println(targetName)
```
示例：获取字符的长度
```go
import (
	"fmt"
	"strconv"
	"unicode/utf8"
)
runeLength := utf8.RuneCountInString(name)
fmt.Println(runeLength)
```
## strings



