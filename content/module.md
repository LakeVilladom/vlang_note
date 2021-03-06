### 模块

V语言是一个非常模块化的语言

模块是代码重用,代码分发的基本单元

程序由模块组成,函数,结构体,常量,枚举等都要在模块中定义

模块内有以下8个一级元素: 常量,枚举,函数,结构体类型,方法,接口,类型,联合体类型

主模块:

```
module main //主模块

fn main() { //在主模块中定义主函数,主函数是程序运行的起点
	
}
```

主模块及其依赖模块,编译后会生成对应平台的单一可执行文件



库模块:

```
module mymodule

fn myfn() {

}
```

库模块编译后会生成对应的库文件,文件扩展名为.o



### 导入模块

导入模块的关键字是import

单行导入:

```c
import os
import strings
```

分组导入:

```c
import (
	os
	strings
)
```

导入模块取别名:

```c
import time as t
import (
	mymodule as mymod
)
```

导入子模块:

```c
import mymodule.submodule
```

模块对应的就是文件系统的目录,子模块对应的就是子目录,通过点号来表示子模块



导入其他模块后,就形成了模块依赖,模块间不允许循环依赖,编译器在编译时会进行检查

如果导入的模块没有被使用:

开发模式(v run xxx)下,编译器只是警告，仍然继续编译,方便开发调试，而不用去临时注释掉

生产编译模式(v -prod xxx)下，编译器会报错，停止编译



### 定义模块

定义模块的关键字是module

模块对应的就是文件系统的目录

要定义一个新模块,先创建一个模块目录,然后在目录中创建.v的源文件,一个模块目录可以创建多个源文件

模块目录名一定要跟源文件中第一行定义的模块名一致

比如模块目录名为mymodule,那么在目录中的源文件的模块定义也要一样,否则无法编译通过

```
module mymodule
```



我们看一下完整的模块定义和导入的代码:

mymodule.v

```
module mymodule

// pub是函数的访问控制,只有公共的函数才可以被其他模块使用,没有pub的只能在模块内部使用
pub fn say_hi() {
	println('hello from mymodule!')
}
```

 main.v

```
module main

import mymodule //导入

fn main() {
	mymodule.say_hi() //使用
}
```

### 模块的初始化函数

如果在模块中定义了init函数,当模块被导入时,init函数会被自动调用

mymodule.v

```
module mymodule

fn init() {
    println('from init')
}

pub fn my_fn() {
    println('from my_fn')
}
```

main.v

```
module main

import mymodule

fn main() {
    mymodule.my_fn()
}
```

执行结果是:

```
from init
from my_fn
```

同一个模块内只能定义一个init函数,如果在模块中定义了多个init函数,编译会不通过

### 模块目录中的源文件

在模块目录中

后缀为_test.v结尾的是测试文件,v test xxx可以执行测试文件

后缀为 _nix 的源文件,表示linux,unix,mac下才会编译

后缀为_darwin的源文件,表示在mac下才会编译

后缀为_linux的源文件,表示在linux下才会编译

后缀为 _windows的源文件,表示在windows下才会编译



