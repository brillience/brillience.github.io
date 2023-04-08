# Golang极速上手


<!--more-->

## 代码规范

### 1. 为什么需要代码规范

1. 代码规范不是强制的，也就是你不遵循代码规范写出来的代码运行也是完全没有问题的
2. 代码规范目的是方便团队形成一个统一的代码风格，提高代码的可读性，规范性和统一性。本规范将从命名规范，注释规范，代码风格和 Go 语言提供的常用的工具这几个方面做一个说明。
3. 规范并不是唯一的，也就是说理论上每个公司都可以制定自己的规范，不过一般来说整体上规范差异不会很大。

### 2. 代码规范

#### 2.1 命名规范

**命名是代码规范中很重要的一部分，统一的命名规则有利于提高的代码的可读性，好的命名仅仅通过命名就可以获取到足够多的信息。**
**a. 当命名（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；**
**b. 命名如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的**（像面向对象语言中的 private ）

##### 2.1.1 包名：package

保持 package 的名字和目录保持一致，尽量采取有意义的包名，简短，有意义，尽量和标准库不要冲突。包名应该为**小写**单词，不要使用下划线或者混合大小写。

package model package main

##### 2.1.2 文件名

尽量采取有意义的文件名，简短，有意义，应该为**小写**单词，使用**下划线**分隔各个单词。
user_model.go

##### 2.1.3 结构体命名

- 采用驼峰命名法，首字母根据访问控制大写或者小写
- struct 申明和初始化格式采用多行，例如下面：

```go
// 多行申明
type User struct{
	 Username string
	 Email string
}
// 多行初始化
u := User{ Username: "bobby", Email: "bobby@imooc.com", }
```

##### 2.1.4 接口命名

- 命名规则基本和上面的结构体类型
- 单个函数的结构名以 “er” 作为后缀，例如 Reader , Writer 。

type Reader interface { Read(p []byte) (n int, err error) }

##### 2.1.5 变量命名

- 和结构体类似，变量名称一般遵循驼峰法，首字母根据访问控制原则大写或者小写，但遇到特有名词时，需要遵循以下规则：
- 如果变量为私有，且特有名词为首个单词，则使用小写，如 apiClient
- 其它情况都应当使用该名词原有的写法，如 APIClient、repoID、UserID
- 错误示例：UrlArray，应该写成 urlArray 或者 URLArray
- 若变量类型为 bool 类型，则名称应以 Has, Is, Can 或 Allow 开头

var isExist bool var hasConflict bool var canManage bool var allowGitHook bool

##### 2.1.6 常量命名

常量均需使用全部大写字母组成，并使用下划线分词

如果是枚举类型的常量，需要先创建相应类型：

### 2.2 注释

Go 提供 C 风格的/\* \*/块注释和 C ++风格的//行注释。行注释是常态；块注释主要显示为包注释，但在表达式中很有用或禁用大量代码。

- 单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释
- 多行注释也叫块注释，均已以 /_ 开头，并以 _/ 结尾，且不可以嵌套使用，多行注释一般用于包的文档描述或注释成块的代码片段

go 语言自带的 go doc 工具可以根据注释生成文档，生成可以自动生成对应的网站（[golang.org](https://link.zhihu.com/?target=http%3A//golang.org) 就是使用 godoc 工具直接生成的），注释的质量决定了生成的文档的质量。每个包都应该有一个包注释，在 package 子句之前有一个块注释。对于多文件包，包注释只需要存在于一个文件中，任何一个都可以。包评论应该介绍包，并提供与整个包相关的信息。它将首先出现在 godoc 页面上，并应设置下面的详细文档。

#### 2.2.1 包注释

每个包都应该有一个包注释，一个位于 package 子句之前的块注释或行注释。包如果有多个 go 文件，只需要出现在一个 go 文件中（一般是和包同名的文件）即可。 包注释应该包含下面基本信息(请严格按照这个顺序，简介，创建人，创建时间）：

- 包的基本简介（包名，简介）
- 创建者，格式： 创建人： rtx 名
- 创建时间，格式：创建时间： yyyyMMdd

#### 2.2.2 结构（接口）注释

每个自定义的结构体或者接口都应该有注释说明，该注释对结构进行简要介绍，放在结构体定义的前一行，格式为： 结构体名， 结构体说明。同时结构体内的每个成员变量都要有说明，该说明放在成员变量的后面（注意对齐），实例如下：

#### 2.2.3 函数（方法）注释

每个函数，或者方法（结构体或者接口下的函数称为方法）都应该有注释说明，函数的注释应该包括三个方面（严格按照此顺序撰写）：

- 简要说明，格式说明：以函数名开头，“，”分隔说明部分
- 参数列表：每行一个参数，参数名开头，“，”分隔说明部分
- 返回值： 每行一个返回值

#### 2.2.4 代码逻辑注释

对于一些关键位置的代码逻辑，或者局部较为复杂的逻辑，需要有相应的逻辑说明，方便其他开发者阅读该段代码，实例如下：

#### 2.2.5 注释风格

统一使用中文注释，对于中英文字符之间严格使用空格分隔， 这个不仅仅是中文和英文之间，英文和中文标点之间也都要使用空格分隔，例如：

上面 Redis 、 id 、 DB 和其他中文字符之间都是用了空格分隔。

- 建议全部使用单行注释
- 和代码的规范一样，单行注释不要过长，禁止超过 120 字符。

### 2.3 **import 规范**

import 在多行的情况下，goimports 会自动帮你格式化，但是我们这里还是规范一下 import 的一些规范，如果你在一个文件里面引入了一个 package，还是建议采用如下格式：

如果你的包引入了三种类型的包，标准库包，程序内部包，第三方包，建议采用如下方式进行组织你的包：

有顺序的引入包，不同的类型采用空格分离，第一种实标准库，第二是项目包，第三是第三方包。
在项目中不要使用相对路径引入包：

但是如果是引入本项目中的其他包，最好使用相对路径。

### 2.4 错误处理

- 错误处理的原则就是不能丢弃任何有返回 err 的调用，不要使用 \_ 丢弃，必须全部处理。接收到错误，要么返回 err，或者使用 log 记录下来
- 尽早 return：一旦有错误发生，马上返回
- 尽量不要使用 panic，除非你知道你在做什么
- 错误描述如果是英文必须为小写，不需要标点结尾
- 采用独立的错误流进行处理

## 语言特性

### 1 Go 语言主要特性

- 自动垃圾回收
- 更丰富的内置类型
- 函数多返回值
- 错误处理
- 匿名函数和闭包
- 类型和接口
- 并发编程
- 反射
- 语言交互性

#### 1.1 自动垃圾回收

> 内存泄露的最佳解决方案是在**语言级别引入自动垃圾回收算法**（Garbage  Collection，简称 GC）。

所谓**垃圾回收**，即所有的内存分配动作都会被在运行时记录，同时任何对 该内存的使用也都会被记录，然后垃圾回收器会对所有已经分配的内存进行跟踪监测，一旦发现 有些内存已经不再被任何人使用，就阶段性地回收这些没人用的内存。

使用 Go 语言，系统会自动帮我们判断，并在合适的时候（比如 CPU 相对空闲的时候）进行自动垃圾收集工作。

#### 1.2 更丰富的内置类型

- map （可以认为是字典）
- slice（一种可动态增长的数组）

#### 1.3 函数多返回值

Go 语言革命性地**在静态开发语言阵营中率先提供了多返回值功能**。

#### 1.4 错误处理

Go 语言引入了 3 个关键字用于标准的错误处理流程，这 3 个关键字分别为**defer、panic 和 recover**。Go 语言的错误处理机制可以**大量减少代码量**，让开发者也无需仅仅为了程序安全性而添加大量 一层套一层的 try-catch 语句。

#### 1.5 匿名函数和闭包

在 Go 语言中，**所有的函数也是值类型**，可以作为参数传递。Go 语言支持常规的匿名函数和闭包。

#### 1.6 类型和接口

类型的定义使用**struct**关键字。引入了一个无比强大的**“非侵入式” 接口**的概念。

```go
//定义一个鸟类型
type Bird struct {
 ...
}
//鸟类型的方法
func (b *Bird) Fly() {
 // 以鸟的方式飞行
}

//定义一个IFly接口
type IFly interface {
	Fly()
}

//使用接口
func main() {
    var fly IFly = new(Bird)
    fly.Fly()
}
```

可以看出，虽然 Bird 类型实现的时候，**没有声明与接口 IFly 的关系，但接口和类型可以直接转换**，甚至接口的定义都不用在类型定义之前，这种比较松散的对应关系可以大幅降低因为接口调整而导致的大量代码调整工作。

#### 1.7 并发编程

引入了 goroutine 概念，使用消息传递来共享内存。Go 语言让并发编程变得更加轻盈和安全。

> 通过在函数调用前使用关键字 go，我们即可让该函数以 goroutine 方式执行。**goroutine**是一种比线程更加轻盈、更省资源的**协程**。。Go 语言通过系统的线程来多路派遣这些函数的执行，使得每个用 go 关键字执行的函数可以运行成为一个单位协程。当一个协程阻塞的时候，调度器就会自动把其他协程安排到另外的线程中去执行，从而实现了程序无等待并行化运行。而且调度的开销非常小，一颗 CPU 调度的规模不下于每秒百万次，这使得我们能够创建大量的 goroutine，从而可以很轻松地编写高并发程序，达到我们想要的目的。

使用 CSP(通信顺序进程，Communicating Sequential Process)模型来作为 goroutine 间的推荐通信方式。(在 CSP 模型中，一个并发系统由若干并行运行的顺序进程组成，每个进程不 能对其他进程的变量赋值。进程之间只能通过一对通信原语实现协作。)Go 语言用**channel（通道）** 这个概念来轻巧地实现了 CSP 模型，可以方便地进行跨 goroutine 的通信。

**由于一个进程内创建的所有 goroutine 运行在同一个内存地址空间中，因此如果不同的 goroutine 不得不去访问共享的内存变量，访问前应该先获取相应的读写锁。Go 语言标准库中的 sync 包提供了完备的读写锁功能**

```go
//由两个goroutine进行并行的累加计算，待这两个计算过程都完成后打印计算结果
package main
import "fmt"
func sum(values [] int, resultChan chan int) {
    sum := 0
    for _, value := range values {
        sum += value
    }
    resultChan <- sum // 将计算结果发送到channel中
}
func main() {
    values := [] int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    resultChan := make(chan int, 2)
    go sum(values[:len(values)/2], resultChan)
    go sum(values[len(values)/2:], resultChan)
    sum1, sum2 := <-resultChan, <-resultChan // 接收结果
    fmt.Println("Result:", sum1, sum2, sum1 + sum2)
}
```

#### 1.8 反射

> **反射**(reflection)，通过反射，你可以**获取对象类型的详细信息，并可动态操作对象。**

反射最常见的使用场景是做对象的序列化(serialization，有时候也叫 Marshal & Unmarshal)。

```go
//以利用反射功能列出某个类型中所有成员变量的值
package main
import (
    "fmt"
    "reflect"
)
type Bird struct {
    Name string
    LifeExpectance int
}
func (b *Bird) Fly() {
    fmt.Println("I am flying...")
}
func main() {
    sparrow := &Bird{"Sparrow", 3}
    s := reflect.ValueOf(sparrow).Elem()
    typeOfT := s.Type()
    for i := 0; i < s.NumField(); i++ {
        f := s.Field(i)
        fmt.Printf("%d: %s %s = %v\n", i,typeOfT.Field(i).Name,f.Type(),
                   f.Interface())
    }
}
```

#### 1.9 语言交互性

Cgo 既是语言特性,同时也是一个工具的名称,用于重用现有 C 模块。在 Go 代码中，可以按 Cgo 的特定语法混合编写 C 语言代码，然后 Cgo 工具可以将这些混合的 C 代码提取并生成对于 C 功能的调用包装代码。开发者基本上可以完全忽略这个 Go 语言和 C 语言的边界是如何跨越的。

### 2 hello world

```go
package main
import "fmt"// 我们需要使用fmt包中的Println()函数
func main() {
	fmt.Println("Hello, world. 你好，世界！")
}
```

- package : 每个 Go 源代码文件的开头都是一个 package 声明，表示该 Go 代码所属的包。**包是 Go 语言里 最基本的分发单位，也是工程管理中依赖关系的体现**。要生成 Go 可执行程序，**必须建立一个名字为 main 的包，并且在该包中包含一个叫 main()的函数**（该函数是 Go 可执行程序的执行起点）。
- main() :    **Go 语言的 main()函数不能带参数，也不能定义返回值。**命令行传入的参数在 os.Args 变量 中保存。如果需要支持命令行开关，可使用 flag 包。
- 函数:    **在函数返回时没有被明确赋值的返回值都会被设置为默认 值，比如 result 会被设为 0.0，err 会被设为 nil。**

```go
func Compute(value1 int, value2 float64) (result float64, err error) {
 // 函数体
}
```

#### 2.1 编译并运行

1.  使用这个命令，会将编译、链接和运行 3 个步骤合并为一步，运行完后在当前目录下也看不 到任何中间文件和最终的可执行文件。

```bash
 go run hello.go
```

2.  只生成编译结果而不自动运行。编译结果为 hello 的可执行文件。

```bash
 go build hello.go
```

### 3 工程管理

Go 命令行工具的革命性之处在于彻底消除了工程文件的概念，完全用目录结构和包名来推 导工程结构和构建顺序。

示例：

这样一个程序：

```bash
$ calc help
USAGE: calc command [arguments] ...
The commands are:
sqrt Square root of a non-negative value.
add Addition of two values.
$ calc sqrt 4 # 开根号
2
$ calc add 1 2 # 加法
3
```

工程结构：

```bash
一个正常的工程目录组织应该如下所示：
<calcproj>
├─<src>
 ├─<calc>
 ├─calc.go
 ├─<simplemath>
 ├─add.go
 ├─add_test.go
 ├─sqrt.go
 ├─sqrt_test.go
├─<bin>
├─<pkg>＃包将被安装到此处
```

- 可执行程序，名为 calc，内部只包含一个 calc.go 文件；
- 算法库，名为 simplemath，每个 command 对应于一个同名的 go 文件，比如 add.go。

在上面的结构里，带尖括号的名字表示其为目录。xxx_test.go 表示的是一个对于 xxx.go 的单元 测试，这也是 Go 工程里的命名规则。

```go
//calc.go
package main

import "os" // 用于获得命令行参数os.Args
import "fmt"
import "simplemath"
import "strconv"

var Usage = func() {
	fmt.Println("USAGE: calc command [arguments] ...")
	fmt.Println("\nThe commands are:\n\tadd\tAddition of two values.\n\tsqrt\tSquar root of a non - negative value.")
}

func main() {
	args := os.Args
	if args == nil || len(args) < 2 {
		Usage()
		return
	}
	switch args[0] {
	case "add":
		if len(args) != 3 {
			fmt.Println("USAGE: calc add <integer1><integer2>")
			return
		}
		v1, err1 := strconv.Atoi(args[1])
		v2, err2 := strconv.Atoi(args[2])
		if err1 != nil || err2 != nil {
			fmt.Println("USAGE: calc add <integer1><integer2>")
			return
		}
		ret := simplemath.Add(v1, v2)
		fmt.Println("Result: ", ret)
	case "sqrt":
		if len(args) != 2 {
			fmt.Println("USAGE: calc sqrt <integer>")
			return
		}
		v, err := strconv.Atoi(args[1])
		if err != nil {
			fmt.Println("USAGE: calc sqrt <integer>")
			return
		}
		ret := simplemath.Sqrt(v)
		fmt.Println("Result: ", ret)
	default:
		Usage()
	}
}
```

```go
// add.go
package simplemath
func Add(a int, b int) int {
	return a + b
}
```

```go
// add_test.go
package simplemath

import "testing"

func TestAdd1(t *testing.T) {
	r := Add(1, 2)
	if r != 3 {
		t.Errorf("Add(1, 2) failed. Got %d, expected 3.", r)
	}
}
```

```go
// sqrt.go
package simplemath

import "math"

func Sqrt(i int) int {
	v := math.Sqrt(float64(i))
	return int(v)
}
```

```go
// sqrt_test.go
package simplemath

import "testing"

func TestSqrt1(t *testing.T) {
	v := Sqrt(16)
	if v != 4 {
		t.Errorf("Sqrt(16) failed. Got %v, expected 4.", v)
	}
}
```

1.  将当前的工程的根目录添加到环境变量 GOPATH 中。
2.  生成可执行文件。加入放到当前工程根目录的 bin 文件夹中：

```bash
cd /calcproj
mkdir bin
go build calc
```

我们不需要写 makefile，因为这个工具会替我们分析，知道目标代码的编译结果应该是一个包还是一个可执行文件，并分析 import 语句以了解包的依赖关系，从而在编译 calc.go 之前先把依赖的 simplemath 编译打包好。

由于已经写了测试代码，并且已添加到环境变量 GOPATH 中，所以可以直接测试：

```bash
go test simplemath
```

若需要调试，可直接使用 gdb 调试编译后的可执行文件(gdb version >7.1)，eg：

```bash
gdb calc
```

## 基本语法

### 1 变量

#### 1.1 声明

```go
var v1 int
var v2 string
var v3 [10]int	//数组
var v4 []int	//数组切片
var v5 struct{
    f int
}
var v6 *int		//指针
var v7 map[string] int		//map   key为string value为int
var v8 func(a int) int		// 匿名函数

//将若干个变量声明在一起，避免了重复写var
var (
	v1 int
	v2 string
)
```

#### 1.2 初始化

```go
var v1 int = 10 // 正确的使用方式1
var v2 = 10 // 正确的使用方式2，编译器可以自动推导出v2的类型
v3 := 10 // 正确的使用方式3，编译器可以自动推导出v3的类型
```

PS：出现在:=左侧的变量不应该是已经被声明过的，否则会导致编译错误。

#### 1.3 赋值

支持多重赋值功能：

```go
i,j=j,i
```

#### 1.4 匿名变量

在调用函数时为了获取一个值，却因为该函数返回多个值而不得不定义一堆没用的变量。这个时候就可以使用匿名变量来接收不需要的值。eg：

```bash
func GetName() (firstName, lastName, nickName string) {
	return "May", "Chan", "Chibi Maruko"
}

_,_,nickName := GetName()
```

### 2 常量

Go 语言中，常量是指编译期间就已知且不可改变的值。常量可以是数值类型（包括整型、 浮点型和复数类型）、布尔类型、字符串类型等。

#### 2.1 字面常量

所谓字面常量（literal），是指程序中硬编码的常量。eg：

```go
-12
3.14159265358979323846 // 浮点类型的常量
3.2+12i // 复数类型的常量
true // 布尔类型的常量
"foo" // 字符串常量
```

> Go 语言的字面常量更接近我们自然语言中的常量概念，它是无类型的。只要这个常量在相应类型的值域范围内，就可以作为该类型的常量，比如上面的常量-12，它可以赋值给 int、uint、int32、 int64、float32、float64、complex64、complex128 等类型的变量。

#### 2.2 常量的定义

通过 const 关键字，给字面常量指定一个友好的名字。

```go
const Pi float64 = 3.14159265358979323846
const zero = 0.0 // 无类型浮点常量
const (
 size int64 = 1024
 eof = -1 // 无类型整型常量
)
const u, v float32 = 0, 3 // u = 0.0, v = 3.0，常量的多重赋值
const a, b, c = 3, 4, "foo"
// a = 3, b = 4, c = "foo", 无类型整型和字符串常量
```

> Go 的常量定义可以限定常量类型，但不是必需的。如果定义常量时没有指定类型，那么它 与字面常量一样，是无类型常量。

常量的赋值是一个编译期行为，所以右值不能出现任何需要运行期才能得出结果的表达式。

#### 2.3 预定义常量

Go 语言预定义了这些常量：**true、false 和 iota**。

- iota : iota 比较特殊，可以被认为是一个可被编译器修改的常量，在每一个 const 关键字出现时被 重置为 0，然后在下一个 const 出现之前，每出现一次 iota，其所代表的数字会自动增 1。如果两个 const 的赋值语句的表达式是一样的，那么可以省略后一个赋值表达式。

```go
const ( // iota被重设为0
	c0 = iota // c0 == 0
	c1 = iota // c1 == 1
	c2 = iota // c2 == 2
)
const (
	a = 1 << iota // a == 1 (iota在每个const开头被重设为0)
	b = 1 << iota // b == 2
	c = 1 << iota // c == 4
)
const (
	u         = iota * 42 // u == 0
	v float64 = iota * 42 // v == 42.0 w = iota * 42 // w == 84
)
const x = iota // x == 0 (因为iota又被重设为0了)
const y = iota // y == 0 (同上)


const ( // iota被重设为0
	c0 = iota // c0 == 0
	c1        // c1 == 1
	c2        // c2 == 2
)
const (
	a = 1 << iota // a == 1 (iota在每个const开头被重设为0)
	b             // b == 2
	c             // c == 4
)
```

#### 2.4 枚举

在 const 后跟一对圆括号的方式定义一组常量，这种定义法在 Go 语言中通常用于定义枚举值。

```go
const (
	Sunday = iota
	Monday
	Tuesday
	Wednesday
	Thursday
	Friday
	Saturday
	numberOfDays // 这个常量没有导出 包内私有
)
```

### 3 类型

- 内置基础类型
  - 布尔类型：bool
  - 整型： int8、byte、int16、int、uint、uintptr 等
  - 浮点类型：float32、float64
  - 复数类型：complex64、complex128
  - 字符串：string
  - 字符类型：rune
  - 错误类型：error
- 复合类型
  - 指针（pointer）
  - 数组（array）
  - 切片（slice）
  - 字典（map）
  - 通道（chan）
  - 结构体（struct）
  - 接口（interface）

#### 3.1 布尔类型

布尔类型不能接受其他类型的赋值，不支持自动或强制的类型转换。

#### 3.2 整型

| 类型    | 字节                                               |
| ------- | -------------------------------------------------- |
| int8    | 1                                                  |
| int16   | 2                                                  |
| int64   | 8                                                  |
| int     | 平台相关                                           |
| uint8   | 1                                                  |
| uint16  | 2                                                  |
| uint32  | 4                                                  |
| uint64  | 8                                                  |
| uint    | 平台相关                                           |
| uintptr | 同指针（32 位平台为 4 字节，63 位平台下位 8 字节） |
| int32   | 4                                                  |

整形之间不可以直接值转换，需要做强制类型转换。

```go
var value2 int32
value1:=64
value2=int32(value1)
```

#### 3.3 浮点型

| 类型    | 字节 |
| ------- | ---- |
| float32 | 4    |
| float64 | 8    |

#### 3.4 复数类型

复数实际上由两个实数（在计算机中用浮点数表示）构成，一个表示实部（real），一个表示 虚部（imag）。

```go
var value1 complex64	// 由2个float32构成的复数类型
value1 = 3.2 + 12i
value2 := 3.2 + 12i 	// value2是complex128类型
value3 := complex(3.2, 12) // value3结果同 value2
```

#### 3.5 字符串

字符串不可以被修改，是常量。字符串操作包括：+、len(s)、s[i]等，更多操作，见 strings 包。

```go
str := "Hello,世界"
for i, ch := range str {
 fmt.Println(i, ch)//ch的类型为rune
}

str := "Hello,世界"
n := len(str)
for i := 0; i < n; i++ {
 ch := str[i] // 依据下标取字符串中的字符，类型为byte
 fmt.Println(i, ch)
}
```

#### 3.6 字符类型

| 类型 | 备注                       |
| ---- | -------------------------- |
| byte | UTF-8 字符串的单个字节的值 |
| rune | 代表单个 Unicode 字符      |

#### 3.7 数组

数组就是指一系列同一类型数据 的集合。数组中包含的每个数据被称为数组元素（element），一个数组包含的元素个数被称为数 组的长度。

```go
//声明方式
[32]byte // 长度为32的数组，每个元素为一个字节
[2*N] struct { x, y int32 } // 复杂类型数组
[1000]*float64 // 指针数组
[3][5]int // 二维数组
[2][2][2]float64 // 等同于[2]([2]([2]float64))
```

需要特别注意的是，在 Go 语言中数组是一个值类型（value type）。所有的值类型变量在赋值和作为参数传递时都将产生一次复制动作。**数组的长度在定义之后无法再次修改。**

#### 3.8 数组切片

与数组相比，数组切片多了一个**存储能力**（capacity）的概念，即元素个数和分配的空间可以是两个不同的值。合理地**设置存储能力**的 值，可以**大幅降低数组切片内部重新分配内存和搬送内存块的频率，从而大大提高程序性能**。

数组切片的数据结构可以抽象为以下 3 个变量：

- 一个指向原生数组的指针
- 数组切片中的元素个数
- 数组切片已分配的存储空间

数组切片添加了一系 列管理功能，可以**随时动态扩充存放空间**，并且可以**被随意传递而不会导致所管理的元素被重复 复制**。

创建方法有两种：

- 基于数组
- 直接创建

```go
// 先定义一个数组
 var myArray [10]int = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
 // 基于数组创建一个数组切片
 var mySlice []int = myArray[:5]
 fmt.Println("Elements of myArray: ")
 for _, v := range myArray {
 fmt.Print(v, " ")
 }
 fmt.Println("\nElements of mySlice: ")
 for _, v := range mySlice {
 fmt.Print(v, " ")
 }

//直接创建
//创建一个初始元素个数为5的数组切片，元素初始值为0：
mySlice1 := make([]int, 5)
//创建一个初始元素个数为5的数组切片，元素初始值为0，并预留10个元素的存储空间：
mySlice2 := make([]int, 5, 10)
//直接创建并初始化包含5个元素的数组切片：
mySlice3 := []int{1, 2, 3, 4, 5}
```

- cap()函数,返回的是数组切片分配的空间大小
- len()函数,返回的是数组切片中当前所存储的元素个数。
- append()函数,继续新增元素。
  **注意**：在第二个参数 mySlice2 后面加了三个点，即一个省略号，如果没有这个省 略号的话，会有编译错误，因为按 append()的语义，从第二个参数起的所有参数都是待附加的 元素。因为 mySlice 中的元素类型为 int，所以直接传递 mySlice2 是行不通的。加上省略号相当于把 mySlice2 包含的所有元素打散后传入。

```go
mySlice2 := []int{8, 9, 10}
myslice := make([]int, 5, 10)
myslice = append(myslice, 8, 9, 10)
// 给mySlice后面添加另一个数组切片
mySlice = append(mySlice, mySlice2...)
//上句等价于
mySlice = append(mySlice, 8, 9, 10)
```

- copy()函数,将内容从一个数组切片复制到另一个数组切片。**如果加入的两个数组切片不一样大，就会按其中较小的那个数组切片的元素个数进行 复制。**

```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := []int{5, 4, 3}
copy(slice2, slice1) // 只会复制slice1的前3个元素到slice2中
copy(slice1, slice2) // 只会复制slice2的3个元素到slice1的前3个位置
```

#### 3.9 map

map 是一堆键值对的未排序集合。

```go
package main

import "fmt"

// PersonInfo是一个包含个人详细信息的类型
type PersonInfo struct {
	ID      string
	Name    string
	Address string
}

func main() {
	var personDB map[string]PersonInfo
	personDB = make(map[string]PersonInfo)
	// 往这个map里插入几条数据
	personDB["12345"] = PersonInfo{"12345", "Tom", "Room 203,..."}
	personDB["1"] = PersonInfo{"1", "Jack", "Room 101,..."}
	// 从这个map查找键为"1234"的信息
	person, ok := personDB["1234"]
	// ok是一个返回的bool型，返回true表示找到了对应的数据
	if ok {
		fmt.Println("Found person", person.Name, "with ID 1234.")
	} else {
		fmt.Println("Did not find person with ID 1234.")
	}
}
```

1.  变量声明

```go
var myMap map[string] PersonInfo
```

2.  创建

```go
myMap = make(map[string] PersonInfo)
myMap = make(map[string] PersonInfo, 100)//存储能力为100
//创建并初始化
myMap = map[string] PersonInfo{
 "1234": PersonInfo{"1", "Jack", "Room 101,..."},
}
```

3.  元素赋值

```go
myMap["1234"] = PersonInfo{"1", "Jack", "Room 101,..."}
```

4.  元素删除

```go
delete(myMap, "1234")
```

5.  元素查找

```go
value, ok := myMap["1234"]
if ok { // 找到了
 // 处理找到的value
}
```

### 4 流程控制

流程控制语句的作用：

- 选择
- 循环
- 跳转

go 语言支持的流程控制语句

- 条件语句
- 选择语句
- 循环语句
- 跳转语句

为了满足更丰富的控制需求，Go 语言还添加了如下关键字：**break**、 **continue**和**fallthrough**。

#### 4.1 条件语句

```go
if a < 5 {
 return 0
} else {
 return 1
}
```

- 条件语句不需要使用括号将条件包含起来()
- 无论语句体内有几条语句，花括号{}都是必须存在的
- 左花括号{必须与 if 或者 else 处于同一行
- **在 if 之后，条件语句之前，可以添加变量初始化语句，使用；间隔；**
- 在有返回值的函数中，**不允许将“最终的”return 语句包含在 if...else...结构中， 否则会编译失败**

#### 4.2 选择语句

```go
switch i {
 case 0:
 fmt.Printf("0")
 case 1:
 fmt.Printf("1")
 case 2:
 fallthrough
 case 3:
 fmt.Printf("3")
 case 4, 5, 6:
 fmt.Printf("4, 5, 6")
 default:
 fmt.Printf("Default")
}

switch {
 case 0 <= Num && Num <= 3:
 fmt.Printf("0-3")
 case 4 <= Num && Num <= 6:
 fmt.Printf("4-6")
 case 7 <= Num && Num <= 9:
 fmt.Printf("7-9")
}
```

- 左花括号{必须与 switch 处于同一行；
- **条件表达式不限制为常量或者整数；**
- 单个 case 中，可以出现多个结果选项；
- 与 C 语言等规则相反**，Go 语言不需要用 break 来明确退出一个 case；**
- 只有在 case 中明确**添加 fallthrough 关键字，才会继续执行紧跟的下一个 case；**
- 可以不设定 switch 之后的条件表达式，在此种情况下，整个 switch 结构与多个 if...else...的逻辑作用等同。

#### 4.3 循环语句

Go 语言中的循环语句只支持 for 关键字。

```go
a := []int{1, 2, 3, 4, 5, 6}
for i, j := 0, len(a) – 1; i < j; i, j = i + 1, j – 1 {
 a[i], a[j] = a[j], a[i]
}
```

- 左花括号{必须与 for 处于同一行。
- Go 语言中的 for 循环与 C 语言一样，都允许在循环条件中定义和初始化变量，唯一的区别是，**Go 语言不支持以逗号为间隔的多个赋值语句，必须使用平行赋值的方式来初始化多个变量。**
- Go 语言的 for 循环同样支持 continue 和 break 来控制循环，但是它提供了一个更高级的 break，可以选择中断哪一个循环，如下例( 本例中，break 语句终止的是 JLoop 标签处的外层循环。)：

```go
for j := 0; j < 5; j++ {
    for i := 0; i < 10; i++ {
        if i > 5 {
            break JLoop
        }
        fmt.Println(i)
    }
}
JLoop:
// ...
```

#### 4.4 跳转语句

goto 语句的语义非常简单，就是跳转到本函数内的某个标签，如：

```go
func myfunc() {
	i := 0
HERE:
	fmt.Println(i)
	i++
	if i < 10 {
		goto HERE
	}
}
```

### 5 函数

#### 5.1 不定参数

1. 指定类型的不定参数

```go
func myfunc(args ...int) {
	for _, arg := range args {
	fmt.Println(arg)
	}
}
```

    	**形如...type格式的类型只能作为函数的参数类型存在，并且必须是最后一个参数。它是一 个语法糖（syntactic sugar）。其本质是数组切片。**

2.  不定类型的不定参数
    **如果你希望传任意类型，可以指定类型为 interface{}。用 interface{}传递任意类型数据是 Go 语言的惯例用法。**

```go
func Printf(format string, args ...interface{}) {
 // ...
}
```

```go
func MyPrintf(args ...interface{}) {
	for _, arg := range args {
		switch arg.(type) {
		case int:
			fmt.Println(arg, "is an int value.")
		case string:
			fmt.Println(arg, "is a string value.")
		case int64:
			fmt.Println(arg, "is an int64 value.")
		default:
			fmt.Println(arg, "is an unknown type.")
		}
	}
}
```

#### 5.2 匿名函数与闭包

1.  **匿名函数**是指不需要定义函数名的一种函数实现方式。Go 语言支持随时在代码里定义匿名函数。匿名函数可以直接赋值给一个变量或者直接执行。

```go
f := func(x, y int) int {
 return x + y
}
func(ch chan int) {
 ch <- ACK
} (reply_chan) // 花括号后直接跟参数列表表示函数调用
```

2.  Go 的匿名函数是一个**闭包**。

- 基本概念
  闭包是可以包含**自由（未绑定到特定对象）变量的代码块**，这些**变量**不在这个代码块内或者任何全局上下文中定义，而是**在定义代码块的环境中定义**。
- 闭包的价值
  闭包的价值在于**可以作为函数对象或者匿名函数**，对于类型系统而言，这意味着不仅要表示 数据还要表示代码。
- Go 中的闭包
  闭包的实现确保只要闭包还被使用，那么 被闭包引用的变量会一直存在。

```go
package main

import (
	"fmt"
)

func main() {
	var j int = 5
	a := func() func() {
		var i int = 10
		return func() {
			fmt.Printf("i, j: %d, %d\n", i, j)
		}
	}()
	a()
	j *= 2
	a()
}

/*
结果：
i, j: 10, 5
i, j: 10, 10
*/
```

### 6 错误处理

#### 6.1 error 接口

Go 语言引入了一个关于错误处理的标准模式，即 error 接口。

```go
type error interface {
 Error() string
}
```

处理错误的方式：

```go
if err != nil {
 // 错误处理
} else {
 // 使用返回值n
}
```

如何使用自定义的 error 类型?

1.  定义一个用于承载错误信息的类型。Go 语言中接口的灵活性，你根本不需要从 error 接口继承。

```go
type PathError struct {
	Op string
	Path string
	Err error
}
```

2.  实现 Error()方法

```go
func (e *PathError) Error() string {
	return e.Op + " " + e.Path + ": " + e.Err.Error()
}
```

#### 6.2 defer

函数在退出前执行 defer 指定的操作。一个函数中可以存在多个 defer 语句，因此需要注意的是，defer 语句的调用是遵照 先进后出的原则，即最后一个 defer 语句将最先被执行。

#### \*6.3 panic 、recover

Go 语言引入了两个内置函数**panic()**和**recover()**以**报告和处理运行时错误和程序中的错误场景**。

**panic**和**recover**相当于 python 中的`try except`。

> 当在一个函数执行过程中调用 panic()函数时，正常的函数执行流程将立即终止，但函数中 之前使用 defer 关键字延迟执行的语句将正常展开执行，之后该函数将返回到调用函数，并导致 逐层向上执行 panic 流程，直至所属的 goroutine 中所有正在执行的函数被终止。错误信息将被报告，包括在调用 panic()函数时传入的参数，这个过程称为**错误处理流程**。

> recover()函数用于终止错误处理流程。一般情况下，recover()应该在一个使用 defer 关键字的函数中执行以有效截取错误处理流程。如果没有在发生异常的 goroutine 中明确调用恢复过程（使用 recover 关键字），会导致该 goroutine 所属的进程打印异常信息后直接退出。

```go
package main

import (
    "fmt"
)

func divide() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Printf("Runtime panic caught: %v\n", err)
        }
    }()

    var i = 1
    var j = 0
    k := i / j
    fmt.Printf("%d / %d = %d\n", i, j, k)
}

func main() {
    divide()
    fmt.Println("divide方法调用完毕，回到main函数")
}
```

## 面向对象

> Go 语言对面向对象编程的支持是语言类型系统中的天然组成部分

### 1 类型系统

> 类型系统是指一个语言的类型体系结构

#### 1.1 为类型添加方法

```go
type Integer int
func (a Integer) Less(b Integer) bool {
 return a < b
}
```

关于给类型添加方法的时候，类型的参数是传入指针？还是值传递？如下：

```go
func (a *Integer) Add(b Integer) {
 *a += b
}

func (a Integer) Add(b Integer) {
 a += b
}
```

**这取决于，你是否需要需要修改对象！若需要修改对象，则需要传入指针。类型都是基于值传递的。要想修改变量的值，只能 传递指针**

#### 1.2 结构体

```go
type Rect struct {
x, y float64
width, height float64
}

func (r *Rect) Area() float64 {
return r.width * r.height
}
```

### 2 初始化

```go
rect1 := new(Rect)
rect2 := &Rect{}
rect3 := &Rect{0, 0, 100, 200}
rect4 := &Rect{width: 100, height: 200}
```

- 未进行显式初始化的变量都会被初始化为该类型的零值。例如 bool 类型的零 值为 false，int 类型的零值为 0，string 类型的零值为空字符串。
- 在 Go 语言中没有构造函数的概念，对象的创建通常交由一个全局的创建函数来完成，以 NewXXX 来命名，表示“构造函数”：

```go
func NewRect(x, y, width, height float64) *Rect {
 return &Rect{x, y, width, height}
}
```

### 3 匿名组合

Go 语言也提供了继承，但是采用了组合的文法，所以我们将其称为**匿名组合**。

```go
type Base struct {
 Name string
}
func (base *Base) Foo() { ... }
func (base *Base) Bar() { ... }

type Foo struct {
 Base
 ...
}
func (foo *Foo) Bar() {
 foo.Base.Bar()
 ...
}
```

### 4 接口

> Go 语言在编程哲学上是变革派,是因为 Go 语言的类型系统，更是因为 Go 语言的接口

“侵入式”的主要表现在于实现类需要明确声明自己实现了某个接口。“非侵入式”只需要实现接口的方法，不需要声明。

```go
type File struct {
 // ...
}
func (f *File) Read(buf []byte) (n int, err error)
func (f *File) Write(buf []byte) (n int, err error)
func (f *File) Seek(off int64, whence int) (pos int64, err error)
func (f *File) Close() error

type IFile interface {
 Read(buf []byte) (n int, err error)
 Write(buf []byte) (n int, err error)
 Seek(off int64, whence int) (pos int64, err error)
 Close() error
}
type IReader interface {
 Read(buf []byte) (n int, err error)
}
type IWriter interface {
 Write(buf []byte) (n int, err error)
}
type ICloser interface {
 Close() error
}

var file1 IFile = new(File)
var file2 IReader = new(File)
var file3 IWriter = new(File)
var file4 ICloser = new(File)
```

尽管 File 类并没有从这些接口继承，甚至可以不知道这些接口的存在，但是 File 类实现了 这些接口，可以进行赋值。

非侵入式接口的好处：

- Go 语言的标准库，再也不需要绘制类库的继承树图。
- 实现类的时候，只需要关心自己应该提供哪些方法，不用再纠结接口需要拆得多细才合理。
- 不用为了实现一个接口而导入一个包，因为多引用一个外部的包，就意味着更多的耦合。

> 在 Go 语言中，只要两个接口拥 有相同的方法列表（次序不同不要紧），那么它们就是等同的，可以相互赋值。

#### 4.1   接口查询

```go
var file1 Writer = ...
if file5, ok := file1.(two.IStream); ok {
 ...
}
```

这个 if 语句检查 file1 接口指向的对象实例是否实现了 two.IStream 接口，如果实现了，则执 行特定的代码。

#### 4.2 类型查询

```go
var v1 interface{} = ...
switch v := v1.(type) {
 case int: // 现在v的类型是int
 case string: // 现在v的类型是string
 ...
}
```

### 5 Any 类型

> 由于 Go 语言中任何对象实例都满足空接口 interface{}，所以 interface{}看起来像是可以指向任何对象的 Any 类型。

```go
var v1 interface{} = 1 // 将int类型赋值给interface{}
var v2 interface{} = "abc" // 将string类型赋值给interface{}
var v3 interface{} = &v2 // 将*interface{}类型赋值给interface{}
var v4 interface{} = struct{ X int }{1}
var v5 interface{} = &struct{ X int }{1}
```

## 并发处理

### 1 并发基础

目前几种主流的实现模式：

- 多进程
  多进程是在操作系统层面进行并发的基本模式。同时也是开销最大的模式。
- 多线程
  多线程在大部分操作系统上都属于系统层面的并发模式，也是我们使用最多的 最有效的一种模式。它比多进程的开销小很多，但是其开销依旧比较大，且在高并发模式下，效率会有影响。
- 基于回调的非阻塞/异步 IO
  使用多线程模式会很快耗尽服务器的内存和 CPU 资源。而这 种模式通过事件驱动的方式使用异步 IO，使服务器持续运转，且尽可能地少用线程，降 低开销，它目前在 Node.js 中得到了很好的实践。
- 协程
  协程（Coroutine）本质上是一种用户态线程，不需要操作系统来进行抢占式调度， 且在真正实现寄存于线程中，因此，系统开销极小，可以有效提高线程的任务并发性，而避免多线程的缺点。

### 2 协程

Go 语言在语言级别支持轻量级线程，叫 goroutine。Go 语言标准库提供的所有系统调用操作（当然也包括所有同步 IO 操作），都会出让 CPU 给其他 goroutine。轻量级线程的切换管理不依赖于系统的线程和进程，也不依赖于 CPU 的核心数量。

### 3 goroutine

goroutine 是 Go 语言中的轻量级线程实现，由 Go 运行时（runtime）管理。

```go
func Add(x, y int) {
 z := x + y
 fmt.Println(z)
}

go Add(1,1)
```

> 需要注意的是，如果这个函数有返回值，那么这个 返回值会被丢弃。

```go
package main

import "fmt"

func Add(x, y int) {
	z := x + y
	fmt.Println(z)
}
func main() {
	for i := 0; i < 10; i++ {
		go Add(i, i)
	}
}
```

> Go 程序从初始化 main package 并执行 main()函数开始，当 main()函数返回时，程序退出， 且程序并不等待其他 goroutine（非主 goroutine）结束。

### 4 并发通信

并发编程的难度在于协调，而协调就要通过交流。在工程上，有两种最常见的并发通信模型：共享数据和消息。

- 共享数据
  指多个并发单元分别保持对同一个数据的引用，实现对该数据的共享。被共享的 数据可能有多种形式，比如内存数据块、磁盘文件、网络数据等。在实际工程应用中最常见的无 疑是内存了，也就是常说的共享内存。
- 消息机制
  Go 语言提供的是另一种通信模型，即以消息机制而非共享内存作为通信方式。**消息机制认为每个并发单元是自包含的、独立的个体，并且都有自己的变量，但在不同并发 单元间这些变量不共享。每个并发单元的输入和输出只有一种，那就是消息。**

### 5 channel

channel 是进程内的通信方式，如果需要跨进程通信，我们建议用 分布式系统的方法来解决，比如使用 Socket 或者 HTTP 等通信协议。一个 channel 只能传递一种类型的值，这个类型需要在声明 channel 时指定。

```go
package main

import "fmt"

func Count(ch chan int) {
	ch <- 1
	fmt.Println("Counting")
}
func main() {
	chs := make([]chan int, 10)
	for i := 0; i < 10; i++ {
		chs[i] = make(chan int)
		go Count(chs[i])
	}
	for _, ch := range chs {
		<-ch
	}
}
```

在这个例子中，我们定义了一个包含 10 个 channel 的数组（名为 chs），并把数组中的每个 channel 分配给 10 个不同的 goroutine。在每个 goroutine 的 Add()函数完成后，我们通过 ch <- 1 语 句向对应的 channel 中写入一个数据。在这个 channel 被读取前，这个操作是阻塞的。在所有的 goroutine 启动完成后，我们通过<-ch 语句从 10 个 channel 中依次读取数据。在对应的 channel 写入 数据前，这个操作也是阻塞的。这样，我们就用 channel 实现了类似锁的功能，进而保证了所有 goroutine 完成后主函数才返回。

#### 5.1 基本语法

```go
var ch chan int
var m map[string] chan bool //声明一个map，key是string，value是channel
ch := make(chan int)

//写入数据
ch <- value
//读出数据
value := <-ch
```

> 向 channel 写入数据通常会导致程序阻塞，直到有其他 goroutine 从这个 channel 中读取数据。如果 channel 之前没有写入数据，那么从 channel 中读取数据也会导致程序阻塞，直到 channel 中被写入数据为止。

经典错误：

```go
package main

import "fmt"

func main(){
	var msg chan int
	msg = make(chan int)
	msg<- 1
	data := <- msg
	fmt.Println(data)
}
// 执行情况：fatal error: all goroutines are asleep - deadlock!
```

> 该错误很经典！因为申请的是一个无缓冲的通道！该通道写入数据后会导致所在线程阻塞，直到其他线程对该通道读操作后，那个被阻塞的线程才会被继续执行。所以上述代码，第 9 行及其之后的代码将不会被执行。

```go
package main

import "fmt"

func main(){
	var msg chan int
	msg = make(chan int)
	go func() {
		data := <- msg
		fmt.Println(data)
	}()
	msg<- 1
}
//执行情况：啥也不会输出，正常结束
```

#### 5.2 select

通过调用 select()函数来监控一系列的文件句柄，一旦其中一个文件句柄发生了 IO 动作，该 select()调用就会被返回。后来该机制也被用于 实现高并发的 Socket 服务器程序。Go 语言直接在语言级别支持 select 关键字，用于处理异步 IO 问题。

> select 有比较多的 限制，其中最大的一条限制就是每个 case 语句里必须是一个 IO 操作。

```go
select {
	case <-chan1:
	// 如果chan1成功读到数据，则进行该case处理语句
	case chan2 <- 1:
		// 如果成功向chan2写入数据，则进行该case处理语句
	default:
		// 如果上面都没有成功，则进入default处理流程
	}
```

> select 的每个 case 语句都必须是一个面向 channel 的操作。

#### 5.3 缓冲机制

给 channel 带上缓冲， 从而达到消息队列的效果。要创建一个带缓冲的 channel，其实也非常容易：

```go
c := make(chan int, 1024)
```

**在调用 make()时将缓冲区大小作为第二个参数传入即可，比如上面这个例子就创建了一个大小 为 1024 的 int 类型 channel，即使没有读取方，写入方也可以一直往 channel 里写入，在缓冲区被 填完之前都不会阻塞。**

#### 5.4 超时机制

在并发编程的通信过程中，最需要处理的就是超时问题，即向 channel 写数据时发现 channel 已满，或者从 channel 试图读取数据时发现 channel 为空。如果不正确处理这些情况，很可能会导 致整个 goroutine 锁死。

使用 channel 时需要小心，比如对于以下这个用法：

```go
i:= <-ch
```

**如果出现了一个错误情况，即永远都没有人往 ch 里写数据，那 么上述这个读取动作也将永远无法从 ch 中读取到数据，导致的结果就是整个 goroutine 永远阻塞并 没有挽回的机会。**

Go 语言没有提供直接的超时处理机制，但我们可以利用 select 机制。虽然 select 机制不是 专为超时而设计的，却能很方便地解决超时问题。因为 select 的特点是只要其中一个 case 已经 完成，程序就会继续往下执行，而不会考虑其他 case 的情况。

```go
timeout := make(chan bool, 1)
go func() {
 time.Sleep(1e9) // 等待1秒钟
 timeout <- true
}()
// 然后我们把timeout这个channel利用起来
select {
 case <-ch:
 // 从ch中读取到数据
 case <-timeout:
 // 一直没有从ch中读取到数据，但从timeout中读取到了数据
}
```

这样使用 select 机制可以避免永久等待的问题，是在 Go 语言开发中避免 channel 通信超时的最有效方法。

#### 5.5 channel 的传递

channel 本身也是一个原生类型,具有可被传递的特性。

示例：定义一系列 PipeData 的数据结构并一起传递给一个函数，就可以达到流式处理数据的目的。

```go
type PipeData struct {
    value   int
    handler func(int) int
    next    chan int
}

func handle(queue chan *PipeData) {
    for data := range queue {
        data.next <- data.handler(data.value)
    }
}
```

#### 5.6 单向 channel

单向 channel 只能用于发送或者接收数据。channel 本身必然是同时支持读写的， 否则根本没法用。在此，我们只是对其增加一些限制。

单向 channel 变量的声明非常简单，如下：

```go
var ch1 chan int // ch1是一个正常的channel，不是单向的
var ch2 chan<- float64// ch2是单向channel，只用于写float64数据
var ch3 <-chan int // ch3是单向channel，只用于读取int数据
```

channel 是一个原生类型，因此不仅 支持被传递，还支持类型转换。

初始化：

```go
ch4 := make(chan int)
ch5 := <-chan int(ch4) // ch5就是一个单向的读取channel
ch6 := chan<- int(ch4) // ch6 是一个单向的写入channel
```

**基于 ch4，我们通过类型转换初始化了两个单向 channel：单向读的 ch5 和单向写的 ch6。**

#### 5.7 关闭 channel

关闭 channel 非常简单，直接使用 Go 语言内置的 close()函数即可：

```go
close(ch)
```

如何判断一个 channel 是否已经被关 闭？我们可以在读取的时候使用多重返回值的方式：

```go
x,ok := <- ch
```

### 6 多核并行化

下面我们来模拟一个完全可以并行的计算任务：计算 N 个整型数的总和。我们可以将所有整 型数分成 M 份，M 即 CPU 的个数。让每个 CPU 开始计算分给它的那份计算任务，最后将每个 CPU 的计算结果再做一次累加，这样就可以得到所有 N 个整型数的总和：

```go
type Vector []float64

// 分配给每个CPU的计算任务
func (v Vector) DoSome(i, n int, u Vector, c chan int) {
	for ; i < n; i++ {
		v[i] += u.Op(v[i])
	}
	c <- 1 // 发信号告诉任务管理者我已经计算完成了
}

const NCPU = 16 // 假设总共有16核
func (v Vector) DoAll(u Vector) {
	c := make(chan int, NCPU) // 用于接收每个CPU的任务完成信号
	for i := 0; i < NCPU; i++ {
		go v.DoSome(i*len(v)/NCPU, (i+1)*len(v)/NCPU, u, c)
	}
	// 等待所有CPU的任务完成
	for i := 0; i < NCPU; i++ {
		<-c // 获取到一个数据，表示一个CPU计算完成了
	}
	// 到这里表示所有计算已经结束
}
```

这或许可能只在一个核上跑。在 Go 语言升级到默认支持多 CPU 的某个版本之前，我们可以先通过设置环境变量 GOMAXPROCS 的值来控制使用多少个 CPU 核心。具体操作方法是通过直接设置环境变量 GOMAXPROCS 的值，或者在代码中启动 goroutine 之前先调用以下这个语句以设置使用 16 个 CPU 核心：

```go
runtime.GOMAXPROCS(16)
```

到底应该设置多少个 CPU 核心呢，其实**runtime**包中还提供了另外一个函数**NumCPU()**来获 取核心数。

### 7 出让时间片

我们可以在每个 goroutine 中控制何时主动出让时间片给其他 goroutine，这可以使用 runtime 包中的 Gosched()函数实现。

> 如果要比较精细地控制 goroutine 的行为，就必须比较深入地了解 Go 语言开发包中 runtime 包所提供的具体功能。

### 8 同步

Go 语言包中的 sync 包提供了两种锁类型：**sync.Mutex**和**sync.RWMutex**。

当一个 goroutine 获得了 Mutex 后，其他 goroutine 就只能乖乖等 到这个 goroutine 释放该 Mutex。RWMutex 相对友好些，是经典的单写多读模型。

#### 8.1 全局唯一性操作

Go 语言提供了一个 Once 类型来保证全局的唯一性操作，具体代码如下：

```go
var a string
var once sync.Once
func setup() {
 a = "hello, world"
}
func doprint() {
 once.Do(setup)
 print(a)
}
func twoprint() {
 go doprint()
 go doprint()
}
```

once 的 Do()方法可以保证在全局范围内只调用指定的函数一次（这里指 setup()函数），而且所有其他 goroutine 在调用到此语句时，将会先被阻塞，直至全局唯一的 once.Do()调用结束后才继续（继续执行此语句后的语句，此语句不再执行）。

为了更好地控制并行中的原子性操作，sync 包中还包含一个 atomic 子包，它提供了对于一 些基础数据类型的原子操作函数，比如下面这个函数：

```go
func CompareAndSwapUint64(val *uint64, old, new uint64) (swapped bool)
```

就提供了比较和交换两个 uint64 类型数据的操作。这让开发者无需再为这样的操作专门添加 Lock 操作。

#### 8.2 waitGroup

WaitGroup 用于等待一组线程的结束。父线程调用 Add 方法来设定应等待的线程的数量。每个被等待的线程在结束时应调用 Done 方法。同时，主线程里可以调用 Wait 方法阻塞至所有线程结束。 但在使用时，也有一些问题需要注意
通过 WaitGroup 提供的三个函数：Add,Done,Wait，可以轻松实现等待某个协程或协程组完成的同步操作。但在使用时要注意:

- Add 的数量和 Done 的调用数量必须相等。
- WaitGroup 结构一旦定义就不能复制。

WaitGroup 在需要等待多个任务结束再返回的业务来说还是很有用的，但现实中用的更多的可能是，先等待一个协程组，若所有协程组都正确完成，则一直等到所有协程组结束；若其中有一个协程发生错误，则告诉协程组的其他协程，全部停止运行（本次任务失败）以免浪费系统资源。  
该场景 WaitGroup 是无法实现的，那么该场景该如何实现呢，就需要用到通知机制，其实也可以用 channel 来实现，具体的解决办法，请看后续的文章。
这样说来，WaitGroup 的使用场景是有限的。

```go
package main

import (
    "fmt"
	"sync"
)

var wg sync.WaitGroup()


func handle(n int) {
    defer wg.Done()
    fmt.Println(n)
}

func main() {
    for i=0; i<=10; i++{
        wg.Add(1)
        go handle(i)
    }
    wg.Wait()
}
/*
WaitGroup是sync包中提供的，它可以让子协程执行完，主协程再死亡。从而达到优雅退出的效果。
注意：
	1. Add函数：开启几个子协程就需要执行几次该函数。
    2. Done函数：执行几次Add函数就需要执行几次Done函数。
    3. Wait函数：只执行一次，就是在子协程全部执行完成后。
*/
```

#### 8.3 互斥锁读写锁

> 互斥锁和读写锁用于处理高并发时对临界区资源操作的同步问题

`互斥锁`：在某一方法对数据进行操作时，会将该数据进行上锁操作，此时其他任何对该数据进行操作的方法都会进入等待，只有当上锁的方法主动解锁其他方法才能得到对该数据的操作权。
`读写锁`：互斥锁影响了高并发服务的性能。读写锁就是为了解决这类问题的。**写数据时上锁**，其他协程都不可以对该数据进行**读和写；读数据时不上锁**，其他协程都可以读和写。上读锁时，只能读不可写；读锁释放，才可写。

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var rwLock sync.RWMutex
var wg sync.WaitGroup

func read(){
	defer wg.Done()
	rwLock.RLock()
	fmt.Println("读取数据")
	time.Sleep(time.Second)
	fmt.Println("读取成功")
	rwLock.RUnlock()
}

func write(){
	defer wg.Done()
	rwLock.Lock()
	fmt.Println("修改数据")
	time.Sleep(time.Second*10)
	fmt.Println("修改成功")
	rwLock.Unlock()
}

func main() {
	wg.Add(10)
	for i := 0;i<5;i++ {
		go read()
	}
	for i := 0;i<5;i++ {
		go write()
	}
	wg.Wait()
}
```

#### 8.4 通道操作

通道可以完成多个协程的通信，通过灵活使用 各种类型的通道的读写通道操作，完成复杂的同步逻辑。例子如下示例代码所示。

- 无缓冲通道
- 有缓冲通道
- 单向通道
- 双向通道

```go
package main

import (
	"encoding/csv"
	"fmt"
	"os"
	"strconv"
	"sync"
	"time"
)

type Server struct {

}

func (server *Server) Connect() chan []string {

	session := make(chan []string, 128)
    //监听session
	go func(c chan []string) {
		file, _ := os.OpenFile("./test.csv", os.O_CREATE|os.O_TRUNC, 0666)
		defer file.Close()
		writer := csv.NewWriter(file)
		defer file.Close()
		for {
			strs := <-c
			if strs[0] == "CLOSE" {
				writer.Write(strs)
				writer.Flush()
				fmt.Println("结束通信")
				break
			}
			writer.Write(strs)
			writer.Flush()
		}

	}(session)

	fmt.Println("建立连接成功!")
	return session
}
func NewServer() *Server {

	return &Server{
   }
}

type Client struct {
	conn chan []string
}

func NewClient(server *Server) *Client {
    //建立客户端的同时，使用通道建立协程间的通信
	c := server.Connect()
	return &Client{c}
}

func (client *Client) Send(strs []string) {
	client.conn <- strs
}

var wp sync.WaitGroup

func main() {
	server := NewServer()
	client := NewClient(server)
	for i := 0; i < 10000; i++ {
		wp.Add(1)
		go func() {
			defer wp.Done()
			client.Send([]string{"hello", strconv.Itoa(i)})
		}()
	}
	//time.Sleep(time.Duration(1) * time.Second)
	wp.Wait()
	go client.Send([]string{"CLOSE"})
	time.Sleep(time.Duration(1) * time.Second)

}
```

#### 8.5 select

selet 的应用有很多，比如：监控异步 IO、超时处理...

> 如下代码是一个超时处理的 demo，每隔两秒往通道里写数据，select 监控到通道被写入了数据，这就代表已经发生了超时，那么 case 后的语句就可以做一些超时的处理操作。

```go
import "time"
import "fmt"
func main() {
    c1 := make(chan string, 1)
    go func() {
        time.Sleep(time.Second * 2)
        c1 <- "result 1"
    }()
    select {
    case res := <-c1:
        fmt.Println(res)
    case <-time.After(time.Second * 1):
        fmt.Println("timeout 1")
    }
}
```

#### 8.6 context

当我们的程序逻辑较为复杂的时候，比如：主线程调起了一个协程，该协程又调起了另外一个协程，这种较为复杂的并发场景对协程的控制非常重要，及时关闭不必要的协程可以保证程序的性能，避免不必要的资源浪费。这个时候`waitGroup`就不好用了，因为它全局不可复制，对于层级较深的协程，就不太适用了。这个时候，就必须使用`channel`和`select`结合使用了。Golang 标准库已经为我们封装好了这种工具，方便对递归调用较深的协程的并发控制`contex包`。

> Go1.7 加入了一个新的标准库 context，它定义了 Context 类型，专门用来简化 对于处理单个请求的多个 goroutine 之间与请求域的数据、取消信号、截止时间等相关操作，这些操作可能涉及多个 API 调用。 --[李文周的博客](https://www.liwenzhou.com/posts/Go/go_context/)

`context`包的核心就是`Context`接口，其定义如下：

```go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```

这个接口共有 4 个方法：

- `Deadline`方法是获取设置的截止时间的意思，第一个返回式是截止时间，到了这个时间点，Context 会自动发起取消请求；第二个返回值 ok==false 时表示没有设置截止时间，如果需要取消的话，需要调用取消函数进行取消。
- `Done`方法返回一个只读的`chan`，类型为`struct{}`，我们在 goroutine 中，如果该方法返回的 chan 可以读取，则意味着 parent context 已经发起了取消请求，我们通过 Done 方法收到这个信号后，就应该做清理操作，然后退出 goroutine，释放资源。
- `Err`方法返回取消的错误原因，因为什么 Context 被取消。
- `Value`方法获取该 Context 上绑定的值，是一个键值对，所以要通过一个 Key 才可以获取对应的值，这个值一般是线程安全的。但使用这些数据的时候要注意同步，比如返回了一个 map，而这个 map 的读写则要加锁。

  8.6.1 两个内置函数

- Backgroud()
- TODO()

这两个函数会返回内置的实现了`context`接口的`background`和`todo`。它们是我们的代码中的上下文对象的最顶层的`parent Context`，由它们会衍生出更多的上下文对象。
`Backgroud()`主要用于 main 函数、初始化以及测试代码中，作为 Context 这个树结构的最顶层的 Context，也就是根 Context。
`TODO()`，它目前还不知道具体的使用场景，如果我们不知道该使用什么 Context 的时候，可以使用这个。
`background`和`todo`本质上都是`emptyCtx`结构体类型，是一个不可取消，没有设置截止时间，没有携带任何值的 Context。

```go
type emptyCtx int
func (*emptyCtx) Deadline() (deadline time.Time, ok bool) {
    return
}
func (*emptyCtx) Done() <-chan struct{} {
    return nil
}
func (*emptyCtx) Err() error {
    return nil
}
func (*emptyCtx) Value(key interface{}) interface{} {
    return nil
}
```

##### 8.6.2 四个 With 函数

- `WithCancel`
- `WithDeadline`
- `WithTimeout`
- `WithValue`

`**WithCancel**`:返回带有新 Done 通道的父节点的副本。当调用返回的 cancel 函数或当关闭父上下文的 Done 通道时，将关闭返回上下文的 Done 通道，无论先发生什么情况。

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
```

Demo: 示例代码中，gen 函数在单独的 goroutine 中生成整数并将它们发送到返回的通道。 gen 的调用者在使用生成的整数之后需要取消上下文，以免 gen 启动的内部 goroutine 发生泄漏。

```go
func gen(ctx context.Context) <-chan int {
		dst := make(chan int)
		n := 1
		go func() {
			for {
				select {
				case <-ctx.Done():
					return // return结束该goroutine，防止泄露
				case dst <- n:
					n++
				}
			}
		}()
		return dst
	}
func main() {
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel() // 当我们取完需要的整数后调用cancel

	for n := range gen(ctx) {
		fmt.Println(n)
		if n == 5 {
			break
		}
	}
}
```

`**WithDeadline**`:返回父上下文的副本，并将 deadline 调整为不迟于 d。如果父上下文的 deadline 已经早于 d，则 WithDeadline(parent, d)在语义上等同于父上下文。当截止日过期时，当调用返回的 cancel 函数时，或者当父上下文的 Done 通道关闭时，返回上下文的 Done 通道将被关闭，以最先发生的情况为准。

```go
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
```

Demo:定义了一个 50 毫秒之后过期的 deadline，然后我们调用 context.WithDeadline(context.Background(), d)得到一个上下文（ctx）和一个取消函数（cancel），然后使用一个 select 让主程序陷入等待：等待 1 秒后打印 overslept 退出或者等待 ctx 过期后退出。因为 ctx 50 毫秒后就会过期，所以 ctx.Done()会先接收到 context 到期通知，并且会打印 ctx.Err()的内容。

```go
func main() {
	d := time.Now().Add(50 * time.Millisecond)
	ctx, cancel := context.WithDeadline(context.Background(), d)

	// 尽管ctx会过期，但在任何情况下调用它的cancel函数都是很好的实践。
	// 如果不这样做，可能会使上下文及其父类存活的时间超过必要的时间。
	defer cancel()

	select {
	case <-time.After(1 * time.Second):
		fmt.Println("overslept")
	case <-ctx.Done():
		fmt.Println(ctx.Err())
	}
}
```

`**WithTimeout**`： `WithTimeout`返回`WithDeadline(parent, time.Now().Add(timeout))`;** 常用于超时控制。**

```go
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
```

```go
package main

import (
	"context"
	"fmt"
	"sync"

	"time"
)

// context.WithTimeout

var wg sync.WaitGroup

func worker(ctx context.Context) {
LOOP://LOOP仅作用于当前for语句
	for {
		fmt.Println("db connecting ...")
		time.Sleep(time.Millisecond * 10) // 假设正常连接数据库耗时10毫秒
		select {
		case <-ctx.Done(): // 50毫秒后自动调用
			break LOOP
		default:
		}
	}
	fmt.Println("worker done!")
	wg.Done()
}

func main() {
	// 设置一个50毫秒的超时
	ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond*50)
	wg.Add(1)
	go worker(ctx)
	time.Sleep(time.Second * 5)
	cancel() // 通知子goroutine结束
	wg.Wait()
	fmt.Println("over")
}
```

`**WithValue**`：返回父节点的副本，其中包含了与 key 关联的值为 val。 间言之就是，返回一个携带了 K-Value 信息的父节点的副本。

> 仅对 API 和进程间传递请求域的数据使用上下文值，而不是使用它来传递可选参数给函数。所提供的键必须是可比较的，并且不应该是`string`类型或任何其他内置类型，以避免使用上下文在包之间发生冲突。`WithValue`的用户应该为键定义自己的类型。为了避免在分配给`interface{}`时进行分配，上下文键通常具有具体类型`struct{}`。或者，导出的上下文关键变量的静态类型应该是指针或接口。 --[李文周的博客](https://www.liwenzhou.com/posts/Go/go_context/)

```go
func WithValue(parent Context, key, val interface{}) Context
```

```go
package main

import (
	"context"
	"fmt"
	"sync"

	"time"
)

// context.WithValue

type TraceCode string

var wg sync.WaitGroup

func worker(ctx context.Context) {
	key := TraceCode("TRACE_CODE")
	traceCode, ok := ctx.Value(key).(string) // 在子goroutine中获取trace code
	if !ok {
		fmt.Println("invalid trace code")
	}
LOOP:
	for {
		fmt.Printf("worker, trace code:%s\n", traceCode)
		time.Sleep(time.Millisecond * 10) // 假设正常连接数据库耗时10毫秒
		select {
		case <-ctx.Done(): // 50毫秒后自动调用
			break LOOP
		default:
		}
	}
	fmt.Println("worker done!")
	wg.Done()
}

func main() {
	// 设置一个50毫秒的超时
	ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond*50)
	// 在系统的入口中设置trace code传递给后续启动的goroutine实现日志数据聚合
	ctx = context.WithValue(ctx, TraceCode("TRACE_CODE"), "12512312234")
	wg.Add(1)
	go worker(ctx)
	time.Sleep(time.Second * 5)
	cancel() // 通知子goroutine结束
	wg.Wait()
	fmt.Println("over")
}
```

##### 8.6.3 注意事项

- 推荐以参数的方式显示传递 Context
- 以 Context 作为参数的函数方法，应该把 Context 作为第一个参数。
- 给一个函数方法传递 Context 的时候，不要传递 nil，如果不知道传递什么，就使用 context.TODO()
- Context 的 Value 相关方法应该传递请求域的必要数据，不应该用于传递可选参数
- Context 是线程安全的，可以放心的在多个 goroutine 中传递

## 网络编程

### 1 TCP

#### 1.1 TCP Server

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"net"
)

func main() {
	server, err := net.Listen("tcp", "127.0.0.1:5000")
	if err != nil {
		fmt.Println("Error:", err.Error())
	}
	for {
		conn, err := server.Accept()
		if err != nil {
			fmt.Println("Error:", err.Error())

		}
		go func(c net.Conn) {
			defer c.Close()
			for {
				reader := bufio.NewReader(c)
				var receive [128]byte
				n, err := reader.Read(receive[:])
				if err != nil {
					if err != io.EOF {
						fmt.Println("Error:", err)

					}
					break
				}
				fmt.Println("receive:", string(receive[:n]))
				c.Write([]byte("你好"))
			}

		}(conn)
	}

}
```

#### 1.2 TCP Client

```go
func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:5000")
	if err != nil {
		fmt.Println("Error:", err.Error())
	}
	defer conn.Close()
	_, err = conn.Write([]byte("hello"))
	if err != nil {
		fmt.Println("Error:", err.Error())
	}
	response := [128]byte{}
	n, err := conn.Read(response[:])
	if err != nil {
		fmt.Println("Error:", err.Error())
	}
	fmt.Println("response:", string(response[:n]))

}
```

### 2 UDP

#### 2.1 UDP Server

```go
func main() {
	server, err := net.ListenUDP("udp", &net.UDPAddr{
		IP:   net.IPv4(0, 0, 0, 0),
		Port: 5000,
	})
	if err != nil {
		fmt.Println("Error:", err.Error())
	}
	defer server.Close()
	for {
		var data [1024]byte
		n, addr, err := server.ReadFromUDP(data[:])
		if err != nil {
			fmt.Println("Error:", err.Error())

		}
		fmt.Printf("data:%v addr:%v count:%v\n", string(data[:n]), addr, n)
		_, err = server.WriteToUDP(data[:n], addr) // 发送数据
		if err != nil {
			fmt.Println("write to udp failed, err:", err)
			continue
		}
	}

}
```

### 3 HTTP

#### 3.1 HTTP Server

```go
func ListenAndServe(addr string, handler Handler) error
```

> 该方法用于在指定的 TCP 网络地址 addr 进行监听，然后调用服务端处理程序来处理传入的连 接请求。该方法有两个参数：第一个参数 addr 即监听地址；第二个参数表示服务端处理程序， 通常为空，这意味着服务端调用 http.DefaultServeMux 进行处理，而服务端编写的业务逻 辑处理程序 http.Handle() 或 http.HandleFunc() 默认注入 http.DefaultServeMux 中。

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
		defer request.Body.Close()
		if request.Method == "GET" {
			fmt.Fprint(writer, "Hello", request.RemoteAddr)
			return
		}
		if request.Method == "POST" {
			request.ParseForm() //这里需要先解析一下，才能拿到结果
			params := request.PostForm
			for k, v := range params {
				fmt.Println(k,v)
			}
			return
		}
	})
	err := http.ListenAndServe("0.0.0.0:5000", nil)
	if err != nil {
		fmt.Println("Error:", err.Error())
		return
	}
}
```

#### 3.2 HTTP Client

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"net/url"
)

func MyGetNoParams(url_ string) (responseBody string, err error) {
	resp, err := http.Get(url_)
	if err != nil {
		fmt.Println("Error:", err)
		return "", err
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println("Error:", err)
		return "", err
	}
	return string(body), nil
}

func MyGetParams(url_ string) (responseBody string, err error) {
	params := url.Values{}
	params.Set("wd", "hello")
	u, err := url.ParseRequestURI(url_)
	if err != nil {
		fmt.Println("Error:", err)
		return "", err
	}
	fmt.Println(u.String())      //http://www.baidu.com
	u.RawQuery = params.Encode() //将需要携带的参数信息拼接到当前url的尾部
	fmt.Println(u.String())      //http://www.baidu.com?wd=hello
	return MyGetNoParams(u.String())
}

func MyPost(url_ string) {
	params := url.Values{}
	params.Set("wd", "hello")
	resp, err := http.PostForm(url_, params)
	if err!= nil{
		fmt.Println(err)
	}
	defer resp.Body.Close()
	body,err := ioutil.ReadAll(resp.Body)
	if err!= nil{
		fmt.Println(err)
	}
	fmt.Println(string(body))
}

func main() {
	//resp, err := MyGetParams("http://www.baidu.com")
	//if err != nil {
	//	fmt.Println("Error:", err)
	//	return
	//}
	//fmt.Println(resp)
	MyPost("http://127.0.0.1:5000/")
}
```

### 4 RPC

go 语言进程间通信的方法有两种：HTTP、RPC。

> RPC（Remote Procedure Call，远程过程调用）是一种通过网络从远程计算机程序上请求服 务，而不需要了解底层网络细节的应用程序通信协议。RPC 协议构建于 TCP 或 UDP，或者是 HTTP 之上，允许开发者直接调用另一台计算机上的程序，而开发者无需额外地为这个调用过程编写网 络通信相关代码，使得开发包括网络分布式程序在内的应用程序更加容易。

RPC 采用**客户端—服务器（Client/Server）的工作模式**。请求程序就是一个客户端（Client）， 而服务提供程序就是一个服务器（Server）。**当执行一个远程过程调用时，客户端程序首先发送一 个带有参数的调用信息到服务端，然后等待服务端响应。在服务端，服务进程保持睡眠状态直到 客户端的调用信息到达为止。当一个调用信息到达时，服务端获得进程参数，计算出结果，并向 客户端发送应答信息，然后等待下一个调用。最后，客户端接收来自服务端的应答信息，获得进 程结果，然后调用执行并继续进行。**

- **一个 RPC 服务端可以注册多个不同类型 的对象，但不允许注册同一类型的多个对象。**
- 一个对象中只有满足如下这些条件的方法，才能被 RPC 服务端设置为可供远程访问：
  - 必须是在对象外部可公开调用的方法（首字母大写）；
  - 必须有两个参数，且参数的类型都必须是包外部可以访问的类型或者是 Go 内建支持的类 型；
  - 第二个参数必须是一个指针；
  - 方法必须返回一个 error 类型的值。
    以上四点可以用一行代码表示：`func (t *T) MethodName(argType T1, replyType *T2) error`
    该方法（MethodName）的第一个参数表示由 RPC 客户端传入的参数，第二个参数表示要返 回给 RPC 客户端的结果，该方法最后返回一个 error 类型的值。

#### 4.1 RPC 服务端

RPC 服务端可以通过调用 **rpc.ServeConn 处理单个连接请求**。多数情况下，**通过 TCP 或 是 HTTP 在某个网络地址上进行监听来创建该服务是个不错的选择**。

#### 4.2 RPC 客户端

在 RPC 客户端，Go 的 net/rpc 包提供了便利的 **rpc.Dial()** 和 **rpc.DialHTTP()** 方法 来与指定的 RPC 服务端建立连接。在建立连接之后，Go 的 net/rpc 包允许我们使用同步或者 异步的方式接收 RPC 服务端的处理结果。调用 RPC 客户端的 **Call() 方法则进行同步处理，**这 时候客户端程序按顺序执行，只有接收完 RPC 服务端的处理结果之后才可以继续执行后面的程 序。当调用 RPC 客户端的 **Go() 方法时，则可以进行异步处理**，RPC 客户端程序无需等待服务 端的结果即可执行后面的程序，而当接收到 RPC 服务端的处理结果时，再对其进行相应的处理。 **无论是调用 RPC 客户端的 Call() 或者是 Go() 方法，都必须指定要调用的服务及其方法名称， 以及一个客户端传入参数的引用，还有一个用于接收处理结果参数的指针。**

如果没有明确指定 RPC 传输过程中使用何种编码解码器，默认将使用 Go 标准库提供的 encoding/gob 包进行数据传输。

#### 4.3 RPC 服务端与客户端代码示例

```go
package myRpc

import "errors"

type Args struct {
	A, B int
}
type Quotient struct {
	Quo, Rem int
}
type Arith int

func (t *Arith) Multiply(args *Args, reply *int) error {
	*reply = args.A * args.B
	return nil
}
func (t *Arith) Divide(args *Args, quo *Quotient) error {
	if args.B == 0 {
		return errors.New("divide by zero")
	}
	quo.Quo = args.A / args.B
	quo.Rem = args.A % args.B
	return nil
}
```

##### 4.3.1 RPC 服务端

```go
package main

import (
	"go_learn/myNet/myRpc"
	"log"
	"net"
	"net/http"
	"net/rpc"
)

func main() {
	arith := new(myRpc.Arith)
	rpc.Register(arith)
	rpc.HandleHTTP()
	l, e := net.Listen("tcp", "127.0.0.1:1234")
	if e != nil {
		log.Fatal("listen error:", e)
	}
	http.Serve(l, nil)
}
```

此时，RPC 服务端注册了一个 Arith 类型的对象及其公开方法 Arith.Multiply()和 Arith.Divide()供 RPC 客户端调用。

##### 4.3.2 RPC 客户端

**RPC 在调用服务端提供的方法之前，必须先与 RPC 服务 端建立连接。**

```go
package main

import (
	"fmt"
	"go_learn/myNet/myRpc"
	"log"
	"net/rpc"
)

func main() {
	client, err := rpc.DialHTTP("tcp", "127.0.0.1:1234")
	if err != nil {
		log.Fatal("dialing:", err)
	}
	//同步调用程序顺序执行的方式
	args := &myRpc.Args{7, 8}
	var reply int
	err = client.Call("Arith.Multiply", args, &reply)
	if err != nil {
		log.Fatal("arith error:", err)
	}
	fmt.Printf("Arith: %d*%d=%d", args.A, args.B, reply)

	//异步调用
	//quotient := new(Quotient)
	//divCall := client.Go("Arith.Divide", args, &quotient, nil)
	//replyCall := <-divCall.Done
}
```

### 5 Gob

> Gob 是 Go 的一个序列化数据结构的编码解码工具，在 Go 标准库中内置 encoding/gob 包 以供使用。一个数据结构使用 Gob 进行序列化之后，**能够用于网络传输**。与 JSON 或 XML 这种 基于文本描述的数据交换语言不同，Gob 是**二进制编码的数据流**，并且 Gob 流是可以自解释的， 它在保证高效率的同时，也具备完整的表达能力。

由于 Gob 仅局 限于使用 Go 语言开发的程序，这意味着我们只能用 Go 的 RPC 实现进程间通信，我们用 Go 编写的 RPC 服务端（或客户端），可能更希望它是通用的，与语言无关的，无 论是 Python 、 Java 或其他编程语言实现的 RPC 客户端，均可与之通信。

### 6 JSON 处理

- Go 语言的大多数数据类型都可以转化为有效的 JSON 文本，但 channel、complex 和函数这几种 类型除外。
- 如果转化前的数据结构中出现指针，那么将会转化指针所指向的值，如果指针指向的是零值， 那么 null 将作为转化后的结果输出。
- 字符串将以 UTF-8 编码转化输出为 Unicode 字符集的字符串，特殊字符比如<将会被转义为 \u003c
- 如果 JSON 中的字段在 Go 目标类型中不存在，json.Unmarshal()函数在解码过程中会丢弃 该字段。
- 目标类型中不可被导出的私有字段（非首 字母大写）将不会受到解码转化的影响。

#### 6.1 解码未知结构的 JSON 数据

空接口是通用类型。如果**要解码一段未知结构的 JSON，只需将这段 JSON 数据解码输出到一个空接口即可**。

#### 6.2 JSON 的流式读写

**Go 内建的 encoding/json 包还提供 Decoder 和 Encoder 两个类型，用于支持 JSON 数据的 流式读写，并提供 NewDecoder()和 NewEncoder()两个函数来便于具体实现。**

```go
package main

import (
	"encoding/json"
	"log"
	"os"
)

func main() {
	dec := json.NewDecoder(os.Stdin)
	enc := json.NewEncoder(os.Stdout)
	for {
		var v map[string]interface{}
		if err := dec.Decode(&v); err != nil {
			log.Println(err)
			return
		}
		for k := range v {
			if k != "Title" {
				v[k] = nil, false
			}
		}
		if err := enc.Encode(&v); err != nil {
			log.Println(err)
		}
	}
}
```

## 常用工具库

## 1 字符串操作

```go
func Compare(a, b string) int //按字典顺序比较a和b字符串大小
func Contains(s, substr string) bool //判断字符串s是否包含substr字符串
func ContainsAny(s, chars string) bool //判断字符串s是否包含chars字符串中的任一字符
func ContainsRune(s string, r rune) bool //判断字符串s是否包含unicode码值r
func Count(s, sep string) int //返回字符串s包含字符串sep的个数
func EqualFold(s, t string) bool //判断s和t两个utf8字符串是否相等，忽略大小写
func Fields(s string) []string //将字符串s以空白字符分割，返回一个切片
func FieldsFunc(s string, f func(rune) bool) []string //将字符串s以满足f(r)==true的字符分割，返回一个切片
func HasPrefix(s, prefix string) bool //判断字符串s是否有前缀字符串prefix
func HasSuffix(s, suffix string) bool //判断字符串s是否有前缀字符串suffix
func Index(s, sep string) int //返回字符串s中字符串sep首次出现的位置
func IndexAny(s, chars string) int //返回字符串chars中的任一unicode码值r在s中首次出现的位置
func IndexByte(s string, c byte) int //返回字符串s中字符c首次出现位置
func IndexFunc(s string, f func(rune) bool) int //返回字符串s中满足函数f(r)==true字符首次出现的位置
func IndexRune(s string, r rune) int //返回unicode码值r在字符串中首次出现的位置
func Join(a []string, sep string) string //将a中的所有字符串连接成一个字符串，使用字符串sep作为分隔符
func LastIndex(s, sep string) int //返回字符串s中字符串sep最后一次出现的位置
func LastIndexAny(s, chars string) int //返回字符串s中任意一个unicode码值r最后一次出现的位置
func LastIndexByte(s string, c byte) int //返回字符串s中字符c最后一次出现的位置
func LastIndexFunc(s string, f func(rune) bool) int //返回字符串s中满足函数f(r)==true字符最后一次出现的位置
func Map(mapping func(rune) rune, s string) string //将字符串s中的每个字符r按函数mapping(r)的规则转换并返回
func Repeat(s string, count int) string //将字符串s重复count次返回
func Replace(s, old, new string, n int) string //替换字符串s中old字符为new字符并返回，n<0是替换所有old字符串
func Split(s, sep string) []string //将字符串s以sep作为分隔符进行分割，分割后字符最后去掉sep
func SplitAfter(s, sep string) []string //将字符串s以sep作为分隔符进行分割，分割后字符最后附上sep
func SplitAfterN(s, sep string, n int) []string //将字符串s以sep作为分隔符进行分割，分割后字符最后附上sep，n决定返回的切片数
func SplitN(s, sep string, n int) []string //将字符串s以sep作为分隔符进行分割，分割后字符最后去掉sep，n决定返回的切片数
func Title(s string) string //将字符串s每个单词首字母大写返回
func ToLower(s string) string //将字符串s转换成小写返回
func ToLowerSpecial(_case unicode.SpecialCase, s string) string //将字符串s中所有字符按_case指定的映射转换成小写返回
func ToTitle(s string) string //将字符串s转换成大写返回
func ToTitleSpecial(_case unicode.SpecialCase, s string) string //将字符串s中所有字符按_case指定的映射转换成大写返回
func ToUpper(s string) string //将字符串s转换成大写返回
func ToUpperSpecial(_case unicode.SpecialCase, s string) string //将字符串s中所有字符按_case指定的映射转换成大写返回
func Trim(s string, cutset string) string //将字符串s中首尾包含cutset中的任一字符去掉返回
func TrimFunc(s string, f func(rune) bool) string //将字符串s首尾满足函数f(r)==true的字符去掉返回
func TrimLeft(s string, cutset string) string //将字符串s左边包含cutset中的任一字符去掉返回
func TrimLeftFunc(s string, f func(rune) bool) string //将字符串s左边满足函数f(r)==true的字符去掉返回
func TrimPrefix(s, prefix string) string //将字符串s中前缀字符串prefix去掉返回
func TrimRight(s string, cutset string) string //将字符串s右边包含cutset中的任一字符去掉返回
func TrimRightFunc(s string, f func(rune) bool) string //将字符串s右边满足函数f(r)==true的字符去掉返回
func TrimSpace(s string) string //将字符串s首尾空白去掉返回
func TrimSuffix(s, suffix string) string //将字符串s中后缀字符串prefix去掉返回
```

## 2 文件操作

- 使用 io/ioutil 进行读写操作
- 使用 OS 进行读写操作

### 2.1 io/ioutil 进行读写操作

读文件：

```go
package main

import (
	"fmt"
	"io/ioutil"
)

func main() {

	b, err := ioutil.ReadFile("e:/tt.txt")
	if err != nil {
		fmt.Println(err)
	}

	fmt.Println(b)
	str := string(b)
	fmt.Println(str)
}
```

写文件：

```go
package main

import (
	"io/ioutil"
)

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {
	d1 := []byte("hello\ngo\n")
	err := ioutil.WriteFile("e:tt2.txt", d1, 0644)
	check(err)
}
```

### 2.2 使用 OS 读写文件

带缓冲的大文件读取：

```go
var path = "..."
var savePath = "..."
file, err := os.Open(path)
defer file.Close()
if err != nil {
    log.Println("文件打开失败!")
}
buf := bufio.NewReader(file)
for {
    line, err := buf.ReadBytes('\n')
    if err == io.EOF {
        log.Println("读文件结束！")
        break
    }
    if err != nil {
        log.Println("读文件失败:", err)
    }
}
```

写文件：

```go
file, err := os.OpenFile("test.txt", os.O_WRONLY | os.O_CREATE | os.O_APPEND, 0600)
if err != nil {
    fmt.Println("打开文件失败; err=",err)
    return
}
defer file.Close()

// 使用缓冲的方式写字符串到文件
writer := bufio.NewWriter(file)
for i:=0; i<3; i++ {
    if _, err := writer.WriteString("Hello 北京"+"\n"); err != nil {
        fmt.Println("文件写入失败, err=", err)
        break
    }
}
// 使用缓冲的方式写byte切片到文件
sli := []byte("Hello 北京\n")
for i:=0; i< 3; i++ {
    if _, err := writer.Write(sli); err != nil {
        fmt.Println("文件写入失败, err=", err)
        break
    }
}
writer.Flush()
```

## 3 Json

参考: [golang 开源 json 库使用笔记](https://segmentfault.com/a/1190000021476347)

- encoding/json, 官方自带的, 文档最多, 易用性差, 性能差
- go-simplejson, gabs, jason 等衍生包, 简单且易用, 易于阅读, 便于维护, 但性能最差
- easyjson, ffjson 此类包, 适合固定结构的 json, 易用性一般, 维护成本高, 性能特别好
- jsonparser 适合动态和固定结构的 json, 简单且易用, 维护成本低, 性能极好

### 3.1 encoding/json

```go
func Marshal(v interface{}) ([]byte, error) // 将Go中数据类型转换为 json 字符串的字节切片
func Unmarshal(data []byte, v interface{}) error // 反序列化json字符串
func Indent(dst *bytes.Buffer, src []byte, prefix, indent string) error //缩进显示json字符串
func Compact(dst *bytes.Buffer, src []byte) error //压缩无效空白符
```

序列化：

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Student struct {
	Id    int			`json:"id"`
	Name  string		`json:"name"`
	Hobby []Hobby
}

type Hobby struct {
	Name     string
	describe string
}

func main() {
	var s0 = Student{
		Id: 10001,
		Name: "张三",
		Hobby: []Hobby{
			{
				Name:     "游泳",
				describe: "游泳。。。。",
			},
			{
				Name:     "钓鱼",
				describe: "台钓",
			},
		},
	}
	ret, err := json.Marshal(&s0)
	if err != nil {
		fmt.Println("序列化失败")
		return
	}
	fmt.Println(string(ret))
}
// 输出：{"id":10001,"name":"张三","Hobby":[{"Name":"游泳"},{"Name":"钓鱼"}]}
```

反序列化：

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name  string		`json:"name"`
	Class []string		`json:"class"`
}

func main()  {
	str := `{"10001": {"name": "张三", "class": ["物理","化学","生物"]}}`
	var user01 interface{}
	if err := json.Unmarshal([]byte(str), &user01); err != nil {
		fmt.Println("反序列化失败,err=", err)
		return
	}
	fmt.Printf("type:%T, value:%v\n", user01, user01)
	// 使用类型断言才能取值
	if user, ok := user01.(map[string]interface {}); ok {
		fmt.Printf("type:%T, value:%v\n", user["10001"], user["10001"])
	}
}
```

### 3.2 simplejson 常用方法

常用于不需要创建结构体时，采用链式调用完成处理。

```
# "github.com/bitly/go-simplejson"
1.  Json 结构体
    type Json struct {
        // contains filtered or unexported fields
    }

2.  构造函数
    func NewJson(body []byte) (*Json, error)

3.  从 io.Reader 接口构造 Json对象
    func NewFromReader(r io.Reader) (*Json, error)

4.  断言相关的方法
    func (j *Json) String() (string, error)
    func (j *Json) Int() (int, error)
    func (j *Json) Int64() (int64, error)
    func (j *Json) Uint64() (uint64, error)
    func (j *Json) Bool() (bool, error)
    func (j *Json) Bytes() ([]byte, error)
    func (j *Json) StringArray() ([]string, error)
    func (j *Json) Interface() interface{}
    func (j *Json) Map() (map[string]interface{}, error)
    func (j *Json) Array() ([]interface{}, error)

5.  保证返回指定类型的结果, args 设定默认值
    func (j *Json) MustInt(args ...int) int
    func (j *Json) MustString(args ...string) string
    func (j *Json) MustFloat64(args ...float64) float64
    func (j *Json) MustBool(args ...bool) bool
    func (j *Json) MustStringArray(args ...[]string) []string
    func (j *Json) MustMap(args ...map[string]interface{}) map[string]interface{}
    func (j *Json) MustArray(args ...[]interface{}) []interface{}

6.  在map类型中，根据key取值
    func (j *Json) Get(key string) *Json

7.  更新json
    func (j *Json) Del(key string)
    func (j *Json) Set(key string, val interface{})

8.  序列化
    func (j *Json) Encode() ([]byte, error)
    func (j *Json) EncodePretty() ([]byte, error)   // 带缩进
```

案例：
json 字符串：

```
{
    "deploy_type":"task",
    "uniqid":"task-01",
    "labels":{
        "deploy_type":"db"
    },
    "callback_url":"https://xxx.xxx.xxx",
    "tasks":[
        {
            "cluster_type":"vm_cluster",
            "deploy_type":"vm_cluster",
            "action":"create",
            "labels":{
                "deploy_type":"cluster"
            },
            "uniqid":"cluster01",
            "name":"cluster01",
            "options":{
                "cadvisor":true
            }
        }
    ]
}
```

#### 3.2.1 取值

```go
func main()  {
	sJson, err := simplejson.NewJson([]byte(str))
	if err != nil {
		fmt.Printf("New json failed, err:%s\n", err.Error())
		return
	}
	// 取labels
	res1 := sJson.Get("labels").Get("deploy_type").MustString("null")
	tmp1, _ := json.Marshal(sJson.Get("tasks").MustArray()[0])  // 返回值为 interface，需要重新序列化
	tmp2, _ := simplejson.NewJson(tmp1)
	res2 := tmp2.Get("labels").Get("deploy_type").MustString()
	fmt.Printf("res1:%s,res2:%s\n", res1, res2)
}
```

#### 3.2.2 修改值

```go
func main()  {
	sJson, err := simplejson.NewJson([]byte(str))
	if err != nil {
		fmt.Printf("New json failed, err:%s\n", err.Error())
		return
	}
	// 删除值
	sJson.Del("tasks")  // 删除task字段
	res1, _:= sJson.EncodePretty()
	fmt.Println(string(res1))
	// 修改值
	sJson.Set("labels", false)  // 修改labels字段
	res2, _:= sJson.EncodePretty()
	fmt.Println(string(res2))
}
```

## 4 csv

### 4.1 流式读文件

```go
func readCsv() {
	file, err := os.Open("./test.csv")
	if err != nil {
		panic(err.Error())
	}
	defer file.Close()
	reader := csv.NewReader(file)
    //若要修改分隔符，则可使用：reader.Comma='\t' ;
	for {
		read, err := reader.Read() //每次读一行，返回一个切片
		if err == io.EOF {
			break
		}
		if err != nil {
			panic(err.Error())
		}
		fmt.Println(read)
	}
}
```

### 4.2 写文件

```go
func writeCsv() {
	file, err := os.OpenFile("./test.csv", os.O_RDWR|os.O_APPEND|os.O_CREATE, 0666) //0666是linux的文件权限设定（0777：-rwxrwxrwx ；0666：-rw-rw-rw-）
	if err != nil {
		panic(err.Error())
	}
	defer file.Close()
	writer := csv.NewWriter(file)
	data := [][]string{{"2", "3", "sdasd"}}
	for _, v := range data {
		err := writer.Write(v)
		if err != nil {
			panic(err.Error())
		}
	}
	writer.Flush()
	fmt.Println("write succ！")
}
```

