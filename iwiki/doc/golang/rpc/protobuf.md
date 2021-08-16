
# protobuf

官网地址：https://developers.google.cn/protocol-buffers/
https://developers.google.com/protocol-buffers/docs/proto3

## 简介
protobuf是谷歌旗下平台无关，语言无关，可扩展的序列化结构数据格式，所以很适合用做数据存储和作为不同应用，不同语言之间的相互通信的数据交换格式，只要实现相同的协议格式即同一 proto 文件被编译成不同的语言版本，加入各自的工程中去。这样不同语言就可以解析其他语言通过 protobuf 序列化的数据。目前官网提供了 C++
Python Java Go 等语言的支持。谷歌在2008年7月7号将其作为开源项目对外公布


## 数据格式的比较

数据格式的交互：xml、json、protobuf 格式比较
1. json：一般的web项目中，最流行的主要还是json。因为浏览器对于json数据支持非常好，有很多的内建的函数支持函
2. xml：在webservice中应用最为广泛，但是相比较json，它的数据更加冗余，因为成对的闭合标签。json使用了键值对的方式，不仅压缩了一定的数据空间，同时也具有可读性
3. protobuf：是后起之秀，是谷歌开源的一种数据格式，适合高性能，对响应速度有要求的数据传输场景。因为protobuf是二进制数据格式，需要编码和解码。数据本身不具有可读性。因此只能反序列化之后得到真正可读的数据

相对于其他的protobuf更具有优势：
1. 序列化后体积相比json和xml很小，适合网络传输
2. 支持跨平台多语言
3. 消息格式升级和兼容性还不错
4. 序列化和反序列化速度很快，快于json的处理速度

## protobuf的优缺点

### 优点
Protobuf有如xml，不过它更小、更快、也更简单。你可以定义自己的数据结构，然后使用代码生成器生成的代码来读写这个数据结构。你甚至可以无需重新部署程序的情况下更新数据结构。只需要使用Protobuf对数据结构进行一次描述，即可利用各种不同的语言或从各种不同的数据流中对你的数据结构化数据轻松读写。它有一个非常棒的特性，即向后兼容性好，人们不必破坏已部署的、依靠老数据格式的程序就可以对数据格式进行升级。Protobuf 语义更清晰，无需类似xml解析器的东西（因为Protobuf编译器会将.protobuf文件编译生成对应的数据访问类以对protobuf数据进行序列化，反序列化操作）使用Protobuf无需学习复杂的文档对象模型，Protobuf的编程模式比较友好，简单易学，同时它拥有了想好的文档和示例，对于喜欢简单事务的人们而言，Protobuf比其他的技术更加有吸引力

### 缺点
Protobuf与Xml相比也有不足之处，它功能简单，无法用来表示复杂的概念。XML已经成为多种行业标准的编写工具，Protobuf只是Google公司内部的使用工具，在通用性上还差很多。由于文本并不适合用来描述数据结构 ，所以Protobuf也不适合用来对基于文本的标记文档（如HTML）建模。另外由于XML具有某种程度上的字解释性，它可以被人直接读取编辑，在这一点上Protobuf不行，它以二进制的方式存储，除非你有.protobuf定义，否则你没法直接读出 Protobuf 任何内容

## protobuf语法
要想使用protobuf必须先得先定义 proto 文件，所以得先熟悉 protobuf 的消息定义的相关语法

### 定义一个消息类型
```
// 版本号
syntax = "proto3";

message PandaRequest {
    string name = 1;
    int32 age = 2;
    repeated string hobby = 3;
}

```

PandaRequest消息格式有3个字段，在消息中承载的数据分别对应每一个字段，其中每个字段都有一个名字和一种类型。文件的第一行指定了你正在使用proto3语法;如果你没有指定这个，编译器会使用proto2。这个指定语法必须是文件的非空非注释的第一个行。在上面的例子中所有字段都是标量类型：两个整形（shenggao和体重），一个string类型(name),Repeated 关键字表示重复的那么在go语言中用切片进行代表，正如上述文件格式，在消息定义中，每个字段都有唯一的一个标识符

### 从.protobuf文件生成了什么？
当用 protobuf 编译器来运行protobuf文件时，编译器将生成所选择语言的代码，这些代码可以操作在.protobuf文件中定义的消息类型，包括获取，设置字段值，将消息序列化到一个输出流中，以及从一个输入流中解析消息。对C++来说，编译器会为每个.protobuf文件生成一个.h文件和一个.cc文件。.protobuf文件中的每个消息有一个对应的类。对于Python来说，有点不太一样Python编译器为.protobuf文件中的每个消息类型生成一个含有静态描述符的模块。该模块与一个元类(metaclass)在运行时(runtime)被用来创建所需的Python数据访问类，对go来说，编译器会为每个消息生成一个.pd.go文件



### 标准的数据类型
一个标量消息字段可以含有一个如下的类型：



### 默认值
当一下消息解析的时候，如果被编码的消息不包含一个特定的元素，被解析的对象锁对应的域被设置为一个默认值，对于不同类型的指定如下：对于strings,默认是一个空string 对于bytes，默认值是一个空的bytes，对于bools,默认是false 对于数值类型，默认是0


### 使用其他消息类型


你可以将其他消息类型用作字符串类型。例如，假设在每一个Personinfo消息中包含Person消息。此时可以在相同的.proto文件中定义一个Result消息类型。然后在Personinfo消息中指定一个Person类型的字段

```


```