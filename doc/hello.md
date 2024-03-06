# Gleam language tour

## hello world

这是一个打印文本“Hello, Joe!”的程序。

它通过使用从 gleam/io 模块导入的 println 函数来实现此目的，该模块是 Gleam 标准库的一部分。

在普通的 Gleam 程序中，该程序将在命令行上使用命令 gleam run 运行，但在本教程中，该程序会在编辑代码时自动编译并运行。

尝试将打印的文本更改为 Hello, Mike! 并看看会发生什么。

```gleam
// Import a Gleam module from the standard library
import gleam/io

pub fn main() {
  // Print to the console
  io.println("Hello, Joe!")
}
```

## Unqualified imports

通常，其他模块中的函数以限定方式使用，模块限定符位于函数名称之前。例如， io.println("Hello!") 。

还可以指定以非限定方式从模块导入的函数列表，例如代码编辑器中的 println 函数。因为它是这样导入的，所以可以简称为 println 。

一般来说，最好使用限定导入，因为这样可以清楚地定义函数的位置，使代码更易于阅读。

```gleam
// Import the module and one of its functions
import gleam/io.{println}

pub fn main() {
  // Use the function in a qualified fashion
  io.println("This is qualified")

  // Or an unqualified fashion
  println("This is unqualified")
}
```

## Type checking

Gleam 拥有强大的静态类型系统，可以在您编写和编辑代码时帮助您发现错误并告诉您在哪里进行更改。

取消注释行 io.println(4) 并查看如何报告编译时错误，因为 io.println 函数仅适用于字符串，不适用于整数。

要修复代码，请将代码更改为调用 io.debug 函数，因为它将打印任何类型的值。

Gleam 没有 null ，没有隐式转换，没有异常，并且始终执行完整的类型检查。如果代码可以编译，您可以相当确信它不存在任何可能导致错误或崩溃的不一致之处。

```gleam
import gleam/io

pub fn main() {
  io.println("My lucky number is:")
  // io.println(4)
  // 👆️ Uncomment this line
}
```

## Int

Gleam 的 Int 类型表示整数。

有用于整数的算术和比较运算符，以及适用于所有类型的相等运算符。

当在 Erlang 虚拟机上运行时，整数没有最大和最小大小。当在 JavaScript 运行时上运行时，整数使用 JavaScript 的 64 位浮点数表示，

gleam/int 标准库模块包含用于处理整数的函数。

```gleam
import gleam/io
import gleam/int

pub fn main() {
  // Int arithmetic
  io.debug(1 + 1)
  io.debug(5 - 1)
  io.debug(5 / 2)
  io.debug(3 * 3)
  io.debug(5 % 2)

  // Int comparisons
  io.debug(2 > 1)
  io.debug(2 < 1)
  io.debug(2 >= 1)
  io.debug(2 <= 1)

  // Equality works for any type
  io.debug(1 == 1)
  io.debug(2 == 1)

  // Standard library int functions
  io.debug(int.max(42, 77))
  io.debug(int.clamp(5, 10, 20))
}
```

## Float

Gleam 的 Float 类型表示非整数的数字。

Gleam 的数值运算符没有重载，因此有专门的运算符用于处理浮点数。

浮点数在 Erlang 和 JavaScript 运行时都表示为 64 位浮点数。浮点行为是它们各自运行时的本机行为，因此它们的确切行为在两个运行时会略有不同。

在 JavaScript 运行时下，超过浮点值的最大（或最小）可表示值将导致 Infinity （或 -Infinity ）。如果您尝试除以两个无穷大，您将得到 NaN 结果。

在 BEAM 上运行时，任何溢出都会引发错误。因此，Erlang 运行时中没有 NaN 或 Infinity 浮点值。

除以零不会溢出，而是定义为零。

gleam/float 标准库模块包含用于处理浮点数的函数。

```gleam
import gleam/io
import gleam/float

pub fn main() {
  // Float arithmetic
  io.debug(1.0 +. 1.5)
  io.debug(5.0 -. 1.5)
  io.debug(5.0 /. 2.5)
  io.debug(3.0 *. 3.5)

  // Float comparisons
  io.debug(2.2 >. 1.3)
  io.debug(2.2 <. 1.3)
  io.debug(2.2 >=. 1.3)
  io.debug(2.2 <=. 1.3)

  // Equality works for any type
  io.debug(1.1 == 1.1)
  io.debug(2.1 == 1.2)

  // Division by zero is not an error
  io.debug(3.14 /. 0.0)

  // Standard library float functions
  io.debug(float.max(2.0, 9.5))
  io.debug(float.ceiling(5.4))
}
```

## Number formats

为了清楚起见，可以在数字中添加下划线。例如， 1000000 可能很难快速阅读，而 1_000_000 可能更容易。

整数可以分别使用 0b 、 0o 和 0x 前缀以二进制、八进制或十六进制格式编写。

浮点数可以用科学记数法来写。

```gleam
import gleam/io

pub fn main() {
  // Underscores
  io.debug(1_000_000)
  io.debug(10_000.01)

  // Binary, octal, and hex Int literals
  io.debug(0b00001111)
  io.debug(0o17)
  io.debug(0xF)

  // Scientific notation Float literals
  io.debug(7.0e7)
  io.debug(3.0e-4)
}
```

## Equality

Gleam 具有用于检查相等性的 == 和 != 运算符。

运算符可以与任何类型的值一起使用，但运算符两侧的类型必须相同。

相等性是在结构上进行检查的，这意味着如果两个值具有相同的结构而不是位于相同的内存位置，则它们相等。

```gleam
import gleam/io

pub fn main() {
  io.debug(100 == 100)
  io.debug(1.5 != 0.1)
}
```

## Strings

在 Gleam 中，字符串被写为用双引号括起来的文本，可以跨越多行并包含 unicode 字符。

<> 运算符可用于连接字符串。

支持多种转义序列：

- \" - 双引号
- \\ - 反斜杠
- \f - 换页
- \n - 换行符
- \r - 回车
- \t - 制表符
- \u{...} - Unicode 字符

gleam/string 标准库模块包含用于处理字符串的函数。

```gleam
import gleam/io
import gleam/string

pub fn main() {
  // String literals
  io.debug("👩‍💻 こんにちは Gleam 🏳️‍🌈")
  io.debug(
    "multi
    line
    string",
  )
  io.debug("\u{1F600}")

  // Double quote can be escaped
  io.println("\"X\" marks the spot")

  // String concatenation
  io.debug("One " <> "Two")

  // String functions
  io.debug(string.reverse("1 2 3 4 5"))
  io.debug(string.append("abc", "def"))
}
```

## Bools

Bool 是 True 或 False 。

|| 、 && 和 ! 运算符可用于操作布尔值。

|| 和 && 运算符是短路的，这意味着如果运算符的左侧是 True （对于 || 或 < b4> 对于 && 则不会评估运算符的右侧。

gleam/bool 标准库模块包含用于处理布尔值的函数。

```gleam
import gleam/io
import gleam/bool

pub fn main() {
  // Bool operators
  io.debug(True && False)
  io.debug(True && True)
  io.debug(False || False)
  io.debug(False || True)

  // Bool functions
  io.debug(bool.to_string(True))
  io.debug(bool.to_int(False))
}
```

## Assignments

可以使用 let 将值分配给变量。

变量名可以被稍后的 let 绑定重用，但它们引用的值是不可变的，因此值本身不会以任何方式更改或变异。

```gleam
import gleam/io

pub fn main() {
  let x = "Original"
  io.debug(x)

  // Assign `y` to the value of `x`
  let y = x
  io.debug(y)

  // Assign `x` to a new value
  let x = "New"
  io.debug(x)

  // The `y` still refers to the original value
  io.debug(y)
}
```

## Discard patterns

如果分配了变量但未使用变量，则 Gleam 将发出警告。

如果不想使用变量，则可以在名称前添加下划线前缀，从而消除警告。

尝试将变量名称更改为 score 以查看警告。

```gleam
pub fn main() {
  // This variable is never used
  let _score = 1000
}
```

## Type annotations

Let 赋值可以在名称后面加上类型注释。

类型注释对于文档目的可能很有用，但除了确保注释正确之外，它们不会改变 Gleam 类型检查代码的方式。

通常，Gleam 代码不会有分配的类型注释。

尝试将类型注释更改为不正确的内容以查看编译错误。

```gleam
pub fn main() {
  let _name: String = "Gleam"

  let _is_cool: Bool = True

  let _version: Int = 1
}
```

## Type aliases

类型别名可用于通过不同名称引用类型。为类型赋予别名并不会产生新类型，它仍然是相同的类型。

当使用 pub 关键字时，类型别名是公共的，可以被其他模块引用。

```gleam
import gleam/io

pub type UserId =
  Int

pub fn main() {
  let one: UserId = 1
  let two: Int = 2

  // UserId and Int are the same type
  io.debug(one == two)
}
```

## Blocks

块是用花括号组合在一起的一个或多个表达式。按顺序计算每个表达式并返回最后一个表达式的值。

块内分配的任何变量只能在块内使用。

尝试取消注释 io.debug(degrees) 以查看尝试使用不在范围内的变量时出现的编译错误。

块还可用于更改二元运算符表达式的计算顺序。

- 比 + 结合得更紧密，因此表达式 1 + 2 _ 3 的计算结果为 7。如果应首先计算 1 + 2 以使表达式计算到 9 则表达式可以包装在块中： { 1 + 2 } _ 3 。这类似于其他一些语言中使用括号进行分组。

```gleam
import gleam/io

pub fn main() {
  let fahrenheit = {
    let degrees = 64
    degrees
  }
  // io.debug(degrees) // <- This will not compile

  // Changing order of evaluation
  let celsius = { fahrenheit - 32 } * 5 / 9
  io.debug(celsius)
}
```

## Lists

列表是值的有序集合。

List 是泛型类型，具有表示其所包含值类型的类型参数。整数列表的类型为 List(Int) ，字符串列表的类型为 List(String) 。

列表是不可变的单链表，这意味着它们可以非常有效地在列表前面添加和删除元素。

计算列表的长度或从列表中的其他位置获取元素的成本很高，而且很少这样做。在 Gleam 中很少编写对序列进行索引的算法，但是当编写它们时，列表并不是数据结构的正确选择

```gleam
import gleam/io

pub fn main() {
  let ints = [1, 2, 3]

  io.debug(ints)

  // Immutably prepend
  io.debug([-1, 0, ..ints])

  // Uncomment this to see the error
  // io.debug(["zero", ..ints])

  // The original lists are unchanged
  io.debug(ints)
}
```

## Constants

除了 let 赋值之外，Gleam 还具有常量，这些常量在模块的顶层定义。

常量必须是文字值，不能在其定义中使用函数。

常量对于在整个程序中使用的值可能很有用，允许对它们进行命名并确保每次使用之间的定义没有差异。

使用常量可能比在多个函数中创建相同的值更有效，尽管确切的性能特征将取决于运行时以及是否编译为 Erlang 还是 JavaScript。

```gleam
import gleam/io

const ints: List(Int) = [1, 2, 3]

const floats = [1.0, 2.0, 3.0]

pub fn main() {
  io.debug(ints)
  io.debug(ints == [1, 2, 3])

  io.debug(floats)
  io.debug(floats == [1.0, 2.0, 3.0])
}
```

## Functions

fn 关键字用于定义新函数。

double 和 multiply 函数是在不使用 pub 关键字的情况下定义的。这使得它们成为私有函数，它们只能在该模块内使用。如果另一个模块尝试使用它们，则会导致编译器错误。

与赋值一样，类型注释对于函数参数和返回值是可选的。为了清晰起见并鼓励有意和深思熟虑的设计，对函数使用类型注释被认为是一种很好的做法。

```gleam
import gleam/io

pub fn main() {
  io.debug(double(10))
}

fn double(a: Int) -> Int {
  multiply(a, 2)
}

fn multiply(a: Int, b: Int) -> Int {
  a * b
}
```

## Higher order functions

在 Gleam 中，函数就是值。它们可以分配给变量，传递给其他函数，以及您可以对值执行的任何其他操作。

这里函数 add_one 作为参数传递给 twice 函数。

请注意， fn 关键字还用于描述 twice 作为第二个参数的函数类型。

```gleam
import gleam/io

pub fn main() {
  // Call a function with another function
  io.debug(twice(1, add_one))

  // Functions can be assigned to variables
  let function = add_one
  io.debug(function(100))
}

fn twice(argument: Int, function: fn(Int) -> Int) -> Int {
  function(function(argument))
}

fn add_one(argument: Int) -> Int {
  argument + 1
}
```

## Anonymous functions

除了模块级命名函数之外，Gleam 还具有使用 fn() { ... } 语法编写的匿名函数文字。

匿名函数可以与命名函数互换使用。

```gleam
import gleam/io

pub fn main() {
  // Assign an anonymous function to a variable
  let add_one = fn(a) { a + 1 }
  io.debug(twice(1, add_one))

  // Pass an anonymous function as an argument
  io.debug(twice(1, fn(a) { a * 2 }))
}

fn twice(argument: Int, function: fn(Int) -> Int) -> Int {
  function(function(argument))
}
```

## Function captures

Gleam 有一种简写语法，用于创建接受一个参数并立即使用该参数调用另一个函数的匿名函数：函数捕获语法。

匿名函数 fn(a) { some*function(..., a, ...) } 可以写为 some_function(..., *, ...) ，并将任意数量的其他参数传递给内部函数。下划线 \_ 是最后一个参数的占位符。

```gleam
import gleam/io

pub fn main() {
  // These two statements are equivalent
  let add_one_v1 = fn(x) { add(1, x) }
  let add_one_v2 = add(1, _)

  io.debug(add_one_v1(10))
  io.debug(add_one_v2(10))
}

fn add(a: Int, b: Int) -> Int {
  a + b
}
```

## Generic functions

到目前为止，每个函数的每个参数都只接受一种类型。

例如， twice 函数仅适用于接受和返回整数的函数。这是过度限制的，只要函数和初始值兼容，就应该可以将此函数与任何类型一起使用。

为了启用此 Gleam 支持泛型，也称为参数多态性。

这是通过使用类型变量代替指定具体类型来实现的，该类型变量代表调用函数时使用的任何特定类型。这些类型变量以小写名称书写。

类型变量与 any 类型不同，每次调用函数时它们都会被替换为特定类型。尝试取消注释 twice(10, exclaim) 以查看尝试同时使用类型变量作为 int 和 string 时出现的编译器错误。

```gleam
import gleam/io

pub fn main() {
  let add_one = fn(x) { x + 1 }
  let exclaim = fn(x) { x <> "!" }

  // Invalid, Int and String are not the same type
  // twice(10, exclaim)

  // Here the type variable is replaced by the type Int
  io.debug(twice(10, add_one))

  // Here the type variable is replaced by the type String
  io.debug(twice("Hello", exclaim))
}

// The name `value` refers to the same type multiple times
fn twice(argument: value, function: fn(value) -> value) -> value {
  function(function(argument))
}
```
