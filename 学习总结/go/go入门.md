go语言学习笔记

- 每个 go 程序都是由包组成的，每个程序的入口都是``package main``(包 main)
-  包名与导入路径的最后一个目录一致，如 `import "math/rand"`，则对应使用直接用rand.xxxxx
- 可以用

  ```go
  import a
  import b
  ```
也可以用

  ```go
  import (
      a,
      b
  )
  ```

- 在 Go 中，首字母大写的名称是被导出的，在导入包之后，你只能访问包所导出的名字，任何未导出的名字是不能被包外的代码访问的，Foo 和 FOO 都是被导出的名称。名称 foo 是不会被导出的
- 函数可以没有参数或接受多个参数，无严格意义上的多态
- 类型在变量名之后  `x int`
- 当两个或多个连续的函数命名参数是同一类型，则除了最后一个类型之外，其他都可以省略 `x, y, z int`
- 存在与es6类似的结构赋值，利用改特性可以实现一些hack

  ```go
  func swap(x, y string) (string, string) {
	  return y, x
  }

  func main() {
	  a, b := swap("hello", "world")
	  fmt.Println(a, b)
  }
```

- Go 的返回值可以被命名，并且就像在函数体开头声明的变量那样使用 `func split(sum int) (x, y int)`
- 没有参数的 return 语句返回各个返回变量的当前值。这种用法被称作“裸”返回，谨慎使用此功能，多个变量会导致代码可读性降低
- var 语句定义了一个变量的列表；跟函数的参数列表一样，类型在后面 `var a, b int`
- 变量定义可以包含初始值，每个变量对应一个 `var a, b int = 1, 2`
- 如果初始化是使用表达式，则可以省略类型；变量从初始值中获得类型 `var a, b = 1, 2`
- 在函数中， := 简洁赋值语句在明确类型的地方，可以用于替代 var 定义。
  
  函数外的每个语句都必须以关键字开始（ var 、 func 、等等）， := 结构不能使用在函数外。
  
  ```go
k := 3
c, python, java := true, false, "no!"
```

- Go 的基本类型有Basic types

  ```go
  bool

  string

  int  int8  int16  int32  int64
  uint uint8 uint16 uint32 uint64 uintptr

  byte // uint8 的别名

  rune // int32 的别名
     // 代表一个Unicode码

  float32 float64

  complex64 complex128
  ```

- 不同类型的变量的赋值

  ```go
  var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
  )
  ```

- int，uint 和 uintptr 类型在32位的系统上一般是32位，而在64位系统上是64位。当你需要使用一个整数类型时，你应该首选 int，仅当有特别的理由才使用定长整数类型或者无符号整数类型
- 变量在定义时没有明确的初始化时会赋值为 零值 。

    零值是：
    数值类型为 0 ，
    布尔类型为 false ，
    字符串为 "" （空字符串）。

- 表达式 T(v) 将值 v 转换为类型 T 。

    一些关于数值的转换：
    
    ```go
    var i int = 42
    var f float64 = float64(i)
    var u uint = uint(f)
    ```
    或者，更加简单的形式：
    
    ```go
    i := 42
    f := float64(i)
    u := uint(f)
    ```
    与 C 不同的是 Go 的在不同类型之间的项目赋值时需要显式转换。 试着移除例子中 float64 或 int 的转换看看会发生什么。

- 在定义一个变量却并不显式指定其类型时（使用 := 语法或者 var = 表达式语法）， 变量的类型由（等号）右侧的值推导得出。(类型推导)

  当右值定义了类型时，新变量的类型与其相同：

  ```go
  var i int
  j := i // j 也是一个 int
  ```   

  但是当右边包含了未指名类型的数字常量时，新的变量就可能是 int 、 float64 或 complex128 。 这取决于常量的精度：
  
  ```go
  i := 42           // int
  f := 3.142        // float64
  g := 0.867 + 0.5i // complex128
  ```

- 常量的定义与变量类似，只不过使用 const 关键字。

  常量可以是字符、字符串、布尔或数字类型的值。

  常量不能使用 := 语法定义。
  
- 数值常量是高精度的值 。
  一个未指定类型的常量由上下文来决定其类型。

- for循环，区别于其它语言，go的for循环不加(  )

  ```go
  for i := 0; i < 10; i++ {
		sum += i
  }
  ```
- 循环初始化语句和后置语句都是可选的
  
  ```go
  for ; sum < 1000; {
		sum += sum
	}
  ```
- 一个hack，C 的 while 在 Go 中叫做 for

  ```go
  for sum < 1000 {
		sum += sum
	}
  ```
  
- 就像 for 循环一样，Go 的 if 语句也不要求用 ( ) 将条件括起来，同时， { } 还是必须有的。
- 跟 for 一样， if 语句可以在条件之前执行一个简单语句。
  由这个语句定义的变量的作用域仅在 if 范围之内。
  
  ```go
  if v := math.Pow(x, n); v < lim {
		return v // 不报错
	}
	return v // 报错
  ```
- 在 if 的便捷语句定义的变量同样可以在任何对应的 else 块中使用
- switch 除非以 fallthrough 语句结束，否则分支会自动终止
 
 ```go
 switch os := c; os {
	case "a":
		// ...
	case "b":
		// ...
	default:
		// ...
 ```
 
- 没有条件的 switch 同 switch true 一样。可实现长 if-then-else 链

   ```go
   t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
   ```

- defer 语句会延迟函数的执行直到上层函数返回。
  延迟调用的参数会立刻生成，但是在上层函数返回前函数都不会被调用，有木有像``setTimeout(()=>{...}, 0)``

- 延迟的函数调用被压入一个栈中。当函数返回时， 会按照后进先出的顺序调用被延迟的函数调用
- Go 具有指针！ 指针保存了变量的内存地址。

  ```go
  var p *int

  i := 42
  p = &i
  
  fmt.Println(*p) // 通过指针 p 读取 i
  *p = 21         // 通过指针 p 设置 i
  ```

- 与 C 不同，Go 没有指针运算
- go 存在结构体 struct

  ```go
  type Vertex struct {
	  X int
	  Y int
  }
  
  v := Vertex{1, 2}
  ```
- 结构体字段使用点号来访问

  ```go
  v := Vertex{1, 2}
  v.X = 4
  ```
- 结构体字段可以通过结构体指针来访问。

  通过指针间接的访问是透明的
  
  ```go
  p := &v
  p.X = 3
  ```
  ```go
  var (
	  v1 = Vertex{1, 2}  // 类型为 Vertex
	  v2 = Vertex{X: 1}  // Y:0 被省略
	  v3 = Vertex{}      // X:0 和 Y:0
	  p  = &Vertex{1, 2} // 类型为 *Vertex
)
  ```
  
- 类型 [n]T 是一个有 n 个类型为 T 的值的数组
 
  ```go
  var a [10]int
  ```
  ```go
  s := []int{2, 3, 5, 7, 11, 13}
  ```

- len(s)返回s的长度
- slice 可以包含任意的类型，包括另一个 slice

  ```go
  game := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}
  ```
  
- slice 可以重新切片，创建一个新的 slice 值指向相同的数组，类似python
 
  ```go
  s := []int{2, 3, 5, 7, 11, 13}
 
  s[1:4] // [3 5 7]
  s[:3] // [2 3 5]
  s[4:] //[11 13]
  s[1:1] // []
  ```
 
- slice 由函数 make 创建。这会分配一个全是零值的数组并且返回一个 slice 指向这个数
 
   ```go
   a := make([]int, 5)  // len(a)=5
   ```

- 为了指定容量，可传递第三个参数到 make

  ```go
  b := make([]int, 0, 5) // len(b)=0, cap(b)=5
  b = b[:cap(b)] // len(b)=5, cap(b)=5
  b = b[1:]      // len(b)=4, cap(b)=4
  ```

- 注意！！ slice 的零值是 nil 。
  一个 nil 的 slice 的长度和容量是 0

- Go 提供了一个内建函数 append，向 slice 的末尾添加元素

  ```go
  func append(s []T, vs ...T) []T
  ```
  
  > append 的第一个参数 s 是一个元素类型为 T 的 slice ，其余类型为 T 的值将会附加到该 slice 的末尾
  
  > 如果 s 的底层数组太小，而不能容纳所有值时，会分配一个更大的数组。 返回的 slice 会指向这个新分配的数组
  
- for 循环的 range 格式可以对 slice 或者 map 进行迭代循环

  ```go
  for i, v := range pow {
  }
  ```
  
  > 每次迭代 range 将返回两个值。 第一个是当前下标（序号），第二个是该下标所对应元素的一个拷贝
  
- 可以通过赋值给 _ 来忽略序号和值

  ```go
  for _, value := range pow 
  ```
  
- map 在使用之前必须用 make 来创建；值为 nil 的 map 是空的，并且不能对其赋值

  ```go
  type Vertex struct {
	  Lat, Long float64
  }

  var m map[string]Vertex
  
  map[string]Vertex{
	  "Bell Labs": Vertex{
		  40.68433, -74.39967,
	  },
	  "Google": Vertex{
		  37.42202, -122.08408,
	},
  ```
  
- `delete(m, key)` 删除元素
- `elem, ok = m[key]` ok会获得一个bool值来判断key是否存在
- Go 函数可以是一个闭包。闭包是一个函数值，它引用了函数体之外的变量。 这个函数可以对这个引用的变量进行访问和赋值；换句话说这个函数被“绑定”在这个变量上

  ```go
  func adder() func(int) int {
	  sum := 0
	  return func(x int) int {
		  sum += x
		  return sum
	  }
  }
  func main() {
	  pos, neg := adder(), adder()
	  for i := 0; i < 10; i++ {
		  fmt.Println(
			  pos(i),
			  neg(-2*i),
		  )
	  }
  } 
  ```
  
- go 没有类，但可以在结构体类型上定义方法

  ```go
  type Vertex struct {
	  X, Y float64
  }

  func (v *Vertex) Abs() float64 {
	  return math.Sqrt(v.X*v.X + v.Y*v.Y)
  }

  func main() {
	  v := &Vertex{3, 4}
	  fmt.Println(v.Abs())
  }
  ```

- 可以对**任意类型**定义任意方法，而不仅仅是针对结构体

  ```go
  type MyFloat float64

  func (f MyFloat) Abs() float64 {
	  if f < 0 {
		  return float64(-f)
	  }
	  return float64(f)
  }

  func main() {
	  f := MyFloat(-math.Sqrt2)
	  fmt.Println(f.Abs())
  }
  ```
> 但不能对外部引入包的任意类型定义方法
>
> 有两个原因需要使用指针接收者。首先避免在每个方法调用中拷贝值（如果值类型是大的结构体的话会更有效率）。其次，方法可以修改接收者指向的值

  ```go
  // 尝试修改 Abs 的定义，同时 Scale 方法使用 Vertex 代替 *Vertex 作为接收者
  type Vertex struct {
	  X, Y float64
  }

  func (v *Vertex) Scale(f float64) {
	  v.X = v.X * f
	  v.Y = v.Y * f
  }

  func (v *Vertex) Abs() float64 {
	  return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

  func main() {
	  v := &Vertex{3, 4}
	  fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
	  v.Scale(5)
	  fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
  }
  ```

- 接口类型是由一组方法定义的集合。
  接口类型的值可以存放实现这些方法的任何值
  
  ```go
  type Abser interface {
	  Abs() float64
  }

  func main() {
	  var a Abser
	  f := MyFloat(-math.Sqrt2)
	  v := Vertex{3, 4}

	  a = f  // a MyFloat 实现了 Abser
	  fmt.Println(a)
	  fmt.Println(a.Abs())
	  a = &v // a *Vertex 实现了 Abser

	  // 下面一行，v 是一个 Vertex（而不是 *Vertex）
	  // 所以没有实现 Abser。
	  //a = v
	  fmt.Println(a)
	  fmt.Println(a.Abs())
}

  type MyFloat float64

  func (f MyFloat) Abs() float64 {
	  if f < 0 {
		  return float64(-f)
	  }
	  return float64(f)
  }

  type Vertex struct {
	  X, Y float64
  }

  func (v *Vertex) Abs() float64 {
	  return math.Sqrt(v.X*v.X + v.Y*v.Y)
  }
  ```


- 隐式接口解藕了实现接口的包和定义接口的包：互不依赖。

  因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。

- Go 程序使用 error 值来表示错误状态。

  > error 类型是一个内建接口
  
  ```go
  type error interface {
      Error() string
  }
  ```

- 通常函数会返回一个 error 值，调用的它的代码应当判断这个错误是否等于 nil， 来进行错误处理
- error 为 nil 时表示成功；非 nil 的 error 表示错误

  ```go
  type MyError struct {
	  When time.Time
	  What string
  }

  func (e *MyError) Error() string  {
	  return fmt.Sprintf("at %v, %s",
	    e.When, e.What)
  }

  func run() error {
	  return &MyError{
		  time.Now(),
		  "it didn't work",
	  }
  }

  func main() {
	  if err := run(); err != nil {
		  fmt.Println(err)
	  }
  }
  ```

- 包 http 通过任何实现了 http.Handler 的值来响应 HTTP 请求

  ```go
  package main

  import (
	  "fmt"
	  "log"
	  "net/http"
  )

  type Hello struct{}

  func (h Hello) ServeHTTP(
	  w http.ResponseWriter,
	  r *http.Request) {
	  fmt.Fprint(w, "Hello!")
  }

  func main() {
	  var h Hello
	  err := http.ListenAndServe("localhost:4000", h)
	  if err != nil {
		  log.Fatal(err)
	  }
  }
  ```

- goroutine 是由 Go 运行时环境管理的轻量级线程

  ```go
  func say(s string) {
	  for i := 0; i < 5; i++ {
		  time.Sleep(100 * time.Millisecond)
		  fmt.Println(s)
	  }
  }

  func main() {
	  go say("world") // 开启一个新的 goroutine 执行
	  say("hello")
  }
  ```
  
- goroutine 在相同的地址空间中运行，因此访问共享内存必须进行同步。sync 提供了这种可能，不过在 Go 中并不经常用到，因为有其他的办法
- channel 是有类型的管道，可以用 channel 操作符 <- 对其发送或者接收值

  ```go
  ch <- v    // 将 v 送入 channel ch。
v := <-ch  // 从 ch 接收，并且赋值给 v。
  ```

- 和 map 与 slice 一样，channel 使用前必须创建

  ```go
  ch := make(chan int)
  ```

- 默认情况下，在另一端准备好之前，发送和接收都会阻塞。这使得 goroutine 可以在没有明确的锁或竞态变量的情况下进行同步
  
  ```go
  package main

  import "fmt"

  func sum(a []int, c chan int) {
	  sum := 0
	  for _, v := range a {
	  	  sum += v
	  }
	  c <- sum // 将和送入 c
  }

  func main() {
	  a := []int{7, 2, 8, -9, 4, 0}

	  c := make(chan int)
	  go sum(a[:len(a)/2], c)
	  go sum(a[len(a)/2:], c)
	  x, y := <-c, <-c // 从 c 中获取

	  fmt.Println(x, y, x+y)
  }
  ```

- channel 可以是 带缓冲的。为 make 提供第二个参数作为缓冲长度来初始化一个缓冲 channel
- 向带缓冲的 channel 发送数据的时候，只有在缓冲区满的时候才会阻塞。 而当缓冲区为空的时候接收操作会阻塞

  ```go
  ch := make(chan int, 100)
  ```
  
- 发送者可以 close 一个 channel 来表示再没有值会被发送了
- 接收者可以通过赋值语句的第二参数来测试 channel 是否被关闭
- 只有发送者才能关闭 channel，而不是接收者。向一个已经关闭的 channel 发送数据会引起 panic

  ```go
  package main

  import (
	  "fmt"
  )

  func fibonacci(n int, c chan int) {
	  x, y := 0, 1
	  for i := 0; i < n; i++ {
		  c <- x
		  x, y = y, x+y
	  }
	  close(c)
  }

  func main() {
	  c := make(chan int, 10)
	  go fibonacci(cap(c), c)
	  for i := range c {
		  fmt.Println(i)
	  }
  }
  ```
- select 语句使得一个 goroutine 在多个通讯操作上等待
  select 会阻塞，直到条件分支中的某个可以继续执行，这时就会执行那个条件分支。当多个都准备好的时候，会随机选择一个
  当 select 中的其他条件分支都没有准备好的时候，default 分支会被执行

  ```go
  func fibonacci(c, quit chan int) {
	  x, y := 0, 1
	  for {
		  select {
		  case c <- x:
			  x, y = y, x+y
		  case <-quit:
			  fmt.Println("quit")
			  return
		  }
	  }
  }
  ```
- 互斥，通常使用 _互斥锁_(mutex)_来提供这个限制
- o 标准库中提供了 sync.Mutex 类型及其两个方法：`Lock Unlock`
我们可以通过在代码前调用 `Lock` 方法，在代码后调用 `Unlock` 方法来保证一段代码的互斥执行

- 也可以用 defer 语句来保证互斥锁一定会被解锁

