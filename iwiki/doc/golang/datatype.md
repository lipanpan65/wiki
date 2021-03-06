[toc]

# 数据类型

Go语言的数值类型包含不同大小的整数型、浮点数和负数，每种数值类型都有大小范围以及正负符号。

## 整数类型

| 类型 | 有无符号 | 占用存储空间                     | 范围          | 备注                                                     |
| :--- | :------- | :------------------------------- | :------------ | :------------------------------------------------------- |
| int  | 有       | 32位系统4个字节，64位系统8个字节 | 2<sup>2</sup> |                                                          |
| uint | 无       | 32位系统4个字节，64位系统8个字节 |               |                                                          |
| rune | 有       | int32位一样                      |               | 等价int32，表示一个Unicode码，当要存贮字符时候，选用byte |
| byte | 无       | unit8等价                        |               |                                                          |

示例：整数类型

1. Go中整数类型分为有符号和无符号
2. Go中默认整形是 `int` 类型
3. 查看变量的字节大小 `unsafe.Sizeof()` ，和数据类型 `%T`

```go
import (
	"fmt"
	"unsafe"
)

func main() {
	// 数据类型的查看 %T (type)
	var num1 int = 100
	fmt.Printf("num1的数据类型：%T \n", num1) // num1的数据类型：int 
	// 字节的类型查看
	fmt.Printf("num1占用的字节数：%d", unsafe.Sizeof(num1)) // num1占用的字节数：8
}

```

### 数字类型

| 序号 | 类型和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **uint8** 无符号 8 位整型 (0 到 255)                         |
| 2    | **uint16** 无符号 16 位整型 (0 到 65535)                     |
| 3    | **uint32** 无符号 32 位整型 (0 到 4294967295)                |
| 4    | **uint64** 无符号 64 位整型 (0 到 18446744073709551615)      |
| 5    | **int8** 有符号 8 位整型 (-128 到 127)                       |
| 6    | **int16** 有符号 16 位整型 (-32768 到 32767)                 |
| 7    | **int32** 有符号 32 位整型 (-2147483648 到 2147483647)       |
| 8    | **int64** 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

## 浮点类型

Go语言中提供了两种精度浮点数 `float32` 和 `float64` , 编译器默认声明为 `float64` 小数点类型就是存放小数的，如 `3.14` 

示例：浮点型的占位符 `%v`

```go
var price float32 = 100.02
fmt.Printf("price类型是：%T，值是 %v", price, price)//%T 类型  %v 默认值	
```

### 浮点数的精度

浮点数的形式：`浮点数=符号位+指数位+位数位`

示例：浮点数的基本使用

```go
// 浮点数的基本使用
var priceFood float32 = 11.22 //正数符号
fmt.Println("food_price=", priceFood)
var num2 float32 = -3.4 //负数符号
var num3 float64 = -8.23
fmt.Println("num1=", num2, "num2=", num3)
```

浮点类型的精度丢失

| 类型          | 占用存储空间 | 表数范围             |
| ------------- | ------------ | -------------------- |
| 单精度float32 | 4字节        | -3.403E38~3.403E38   |
| 双精度float64 | 8字节        | -1.798E308~1.798E308 |

示例：浮点类型的精度丢失

```go
//尾数可能丢失，精度缺损
var num4 float32 = -123.11111111105 //精度丢失了
var num5 float64 = -123.11111111105 //float64的精度高于float32
fmt.Println("num3=", num4, "num4=", num5)
//输出结果
//num4= -123.111115 num5= -123.11111111105
```

## 字符型

Go 中如果要存储单个`字符(字母)` ，一般要用 `byte` 保存，普通字符串就是一串固定的长度的字符连接起来的字符序列。Go的字符串是由单个 `字符` 连接起来的也就是说对于传统的字符串 `字符` 组成的，而Go的字符串不同，它是由 `字节` 组成的。Go的字符用 `单引号` 表示 ，字符串用 `双引号` 表示

示例：格式化输出使用 `%c`

```go
// go 中的字符
var woman = 'w'
var man = 'm'
fmt.Println(woman, man) // 119 109
fmt.Printf("男生表示 %c", man)
```

示例：当保存的字符对应码255，应该使用int而不是byte，否则 overflows byte 异常

```go
var boy byte = '男'
fmt.Println(boy) // ./char_main.go:12:6: constant 30007 overflows byte
var girl int = '女'
fmt.Println(girl)
```

示例：Go语言中对字符输出得到的是 `unicode` 码

```go
var c4 int = 22269
fmt.Printf("c4=%c\n", c4)
//输出结果c4=国
```

## 布尔型

Go中一个布尔型只有两种 true 和 false

示例：if 和 for 语句的条件部分都是布尔类型的值，并且==和<等比较操作也会产生布尔型的值

```go
var flag = true
fmt.Println(flag)
fmt.Println("b字节数=", unsafe.Sizeof(flag))
if 2 > 1 {
  fmt.Println("flag is true")
} else {
  fmt.Println("flag is false")
}
```





