# Scala 编程风格规范

Scala 编程的二大难点在于其语言的灵活性和函数式编程的范式思维。无论团队经验如何，一套实用的规范会大大提升个人和团队的工作效率。

## 函数式编程基本原则

函数式编程的习惯养成有赖于思维的转换。思维的转换可以通过行为的改变来形成。下面的原则是函数式编程的十项基本原则：

- 不用 Class 来封装可变数据和操作。数据尽量用 `case class`， 操作用伴生对象或根据操作种类放到不同的 object 里面。有时用到 Functional Object - 用 Class 封装不可变数据和一些方法（最常见的是 `toString`），这种用法是闭包 Closure。
- 不用 `null`， 用 `Option`， `Either` 或 `Try`。
- 不用异常 `Throwable`. 用 `Try` 或 `Either` 来封装产生异常的调用。
- 只用 `val`, 除了内部隐藏使用，不要用 `var`。
- 只用不可变 immutable 集合。
- 除了 Boolean 变量用 `if`, `else`。其它尽量用模式匹配。
- 用 pure function，知道用递归或函数的参数来设置状态。logging 作为唯一允许的副作用可以用在函数里面。
- 使用异步操作，在专门线程池调用阻塞 blocking API。
- 不知道如何做，最好请教有经验的同事。
- 如果有很好的理由和经过资深程序员同意，可以打破这些原则。

## 目录，包和文件名

目录都用小写，如果有多个单词则用横杠分隔, 称为 Kebab Case 名称法，比如 customer-order 。

Package 包名加入前缀：`公司.项目名称`。都是小写。项目名称可能包含二个单词（不要再多了，通过层级应该明确了），就用小写拼接，不加入下划线或其它分隔符，比如 customerorder。根据需要可以有更多层，但是不建议超过 5 层。比如员工业绩报表包名为 `tehang.staffrpt`。

文件名按照 Scala 标准用 Pascal Case， 即首字母大写，比如 `CustomerOrder.scala`.

## 大小

- 一行长度不超过 80（Scalafmt 的缺省值，明确定义）：`maxColumn = 80`。
- 一个函数实现一个目的明确的功能，不要超过 7 个操作，所有操作在同一抽象层 [SLA: Single Leval Abstraction](http://principles-wiki.net/principles:single_level_of_abstraction).
- 一个函数不要超过 30 行，不算注释，包括空行。超过的拆分。
- 一个类不要超过 7 个函数。超过的拆分。
- 一个文件通常只包含一个类及其伙伴对象，100 行以内最好，不要超过 200 行。

## Coding Format

尽可能在 IDE 和 sbt [Scalafmt](https://scalameta.org/scalafmt/) 设置代码规范.

## Scalac 选项

拷贝 [Recommended Scalac Flags For 2.13](https://nathankleyn.com/2019/05/13/recommended-scalac-flags-for-2-13/)。

特别是需要使用 `"-Xfatal-warnings"` 来消除所有告警信息。因为某种原型无法消除的，可以用 [Scala compiler Silencer](https://github.com/ghik/silencer) 来关闭。

## 命名规范

- 多数业务名称都会在多处使用，按产品和业务定义名词已经响应的英文名词。英文名词用于程序的各种文件、包、类已经变量命名。
- DDD（domain driven design）：每个子域有不同的定义，需要分布式维护清晰的定义。可以放入源文件库。
- Scala 源文件名用 PascalCase。比如 `CustomerOrder.scala`。
- 除非循环只有一句话，不要用 `i`。双重或以上循环禁用简写。Use `index` instead of `i`
- 常量名用 PascalCase。比如 `OutputFileTmpPath = "/tmp/"` 。

## 引入

- 引入在文件头部。
- 引入使用具体数据类型，避免通配符。如果引入过多，说明文件太大，应该拆分。
- 大类顺序采用： Scala/Java 标准库、框架库、公司和自己定义的类型三大类。
- 类中应用顺序：引入按字母排序。
- 例外情况：高度相似的引用条目
  - `import scala.concurrent.duration._`。内含高度相似的各种时间单位。如果可能，引用写到函数内部。多个函数引用则写到文件头部。
  - Import 自动生成的数据库表时。通常把相关的数据库表放到一起在 SQL Join 或代码中引用。

## 尽量使用 `final case class` 而不是 `class`

## 类型标注

- 类里所有公共值都需要显式标注数据类型。

## 不用 Null 和 Exception，使用

- `Option` 对于值可能为空
- `Either` 对于自己明白的错误
- `Try` 打包可能抛出异常的调用返回值

## 工具（待确定）

- scapegoat
- wartremover
- scala linter
- scalastyle
- scalafix
