<!-- TOC -->

- [go命令](#go命令)
- [变量](#变量)
- [if语句](#if语句)
- [map](#map)
- [函数make](#函数make)
- [fmt包](#fmt包)
- [bufio包](#bufio包)
- [os包](#os包)
- [strings包](#strings包)
  - [strings.Split函数](#stringssplit函数)
- [io/ioutil包](#ioioutil包)
  - [ioutil.ReadFile函数](#ioutilreadfile函数)
- [比较](#比较)

<!-- /TOC -->
* go语言原生支持Unicode，可以处理全世界任何语言的文本。
  


# go命令
* Go语言提供的工具都通过一个单独的命令go调用
* 子命令
  * run 
    编译一个或多个以.go结尾的源文件，链接库文件，并运行最终生成的可执行文件。
    ```go
    $ go run hello.go
    ```
  * build 
    编译并保存编译结果。生成一个可执行的二进制文件，之后可随时运行，不需任何处理。
    ```go
    $ go build hello.go
    $ ./hello
    ```

    
# 变量
* 函数和包级别的变量可以任意顺序声明，并不影响其被调用
* 短变量声明
  ```go
  input := bufio.NewScanner(os.Stdin)
  ```

# if语句
* 条件两边不加括号
* if语句的else部分是可选的

# map
```go
    counts := make(map[string]int)
```
* map是一个由make函数创建的数据结构的引用
  * 作为参数传递给某函数时，该函数接收这个引用的一份拷贝，被调用函数对map底层数据结构的任何修改，调用者函数都可以通过持有的map引用看到。
* map存储了键/值（key/value）的集合
* 对集合元素，提供常数时间的存、取或测试操作
* 键可以是任意类型，只要其值能用==运算符比较
* 值可以是任意类型
  * map中不含某个键时，其值将被计算为其类型的零值
* map的迭代顺序并不确定，从实践来看，该顺序随机，每次运行都会变化（这种设计能防止程序依赖特定遍历顺序）
* 从功能和实现上说，Go的map类似于
  * Java语言中的HashMap
  * Python语言中的dict
  * Lua语言中的table
  * 通常使用hash实现

# 函数make
* 内置函数make创建空map
    ```go
    counts := make(map[string]int)
    ```
# fmt包
* fmt.Printf函数
  * 对一些表达式产生格式化输出
  * 默认不换行
  * 首个参数是个格式字符串，指定后续参数被如何格式化
  * 各个参数的格式取决于“转换字符”，形式为百分号后跟一个字母
    * Go程序员称这种转换为动词
    * %d表示以十进制形式打印一个整型操作数
    * %x十六进制整数
    * %o八进制整数
    * %b二进制整数
    * %f,%g,%e浮点数：3.141593 3.141592653589793 3.141593e+00
    * %t布尔：true或false
    * %c字符（rune）（Unicode码点）
    * %s表示把字符串型操作数的值展开
    * %q带双引号的字符串"abc"或带单引号的字符'c'
    * %v变量的自然形式（natural format）
    * %T变量的类型
    * %%字面上的百分号标志（无操作数）
  * 代表不可见字符的转义字符
    * 制表符\t
    * 换行符\n
  * 按照惯例，以字母f结尾的格式化函数，如log.Printf和fmt.Errorf，都采用fmt.Printf的格式化准则
  * 以ln结尾的格式化函数，则遵循Println的方式，以跟%v差不多的方式格式化参数，并在最后添加一个换行符
* fmt.Fprintf函数
  ```go
  fmt.Fprintf(os.Stderr, "dup2:%v\n", err)
  //使用Fprintf与表示任意类型默认格式值的动词%v，向标准错误流打印一条信息
  ```

# bufio包
* 使处理输入和输出方便又高效
* Scanner类型是该包最有用的特性之一
  * 读取输入并将其拆成行或单词
  * 通常是处理行形式的输入最简单的方法
  * 使用短变量声明创建bufio.Scanner类型的变量input
    ```go
    input := bufio.NewScanner(os.Stdin)
    //input变量从程序的标准输入中读取内容
    ```
  * 每次调用input.Scan()，即读入下一行，并移除行末的换行符
    * Scan函数在读到一行时返回true，在无输入时返回false
  * 读取的内容可以调用input.Text()得到


# os包
* os.Open函数
  * 返回两个值：
    * 第一个是被打开的文件（*os.File）
    * 第二个是内置error类型的值。
      * 如果err等于内置值nil，那么文件被成功打开（可调用Close关闭该文件，释放资源）
      * else则错误处理

# strings包
## strings.Split函数
* 把字符串切割成子串的切片

# io/ioutil包
## ioutil.ReadFile函数
* 读取指定文件的全部内容
* 返回一个字节切片

# 比较
* bufio.Scanner、ioutil.ReadFile、ioutil.WriteFile
  * 实现上，它们都使用*os.File的Read和Write方法