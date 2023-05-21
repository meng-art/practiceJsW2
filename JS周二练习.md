## 学习内容：JS 变量、作用域与内存 + 基本引用类型

### 一、JavaScript有哪些类型

​         JS有八种数据类型，未定义（Undefined）、空（Null）、数字（Number）、字符串（String）、布尔值（Boolean）、符号（Symbol）、任意大整数（BigInt）、对象（Object）

1. Undefined：他只有一个值就是Undefined，任何变量在未赋值前均是Undefined。
2. Null：只有一个值 null，表示变量被置为空对象，表示未知的值。
3. Number： 表示数字，包括整数及浮点数。
4. String：表示零或多个 16 位 Unicode 字符序列，字符串是不可变的，要修改 某个变量中的字符串值，必须先销毁原始的字符串。
5. Boolean：逻辑类型，仅有true和false。
6. Symbol：符号是原始值，且符号实例是唯一、不可变的。
7. Blight：用于任意长度的整数。
8. Object：派生其他对象的基类。

### 二、基本类型和引用类型的区别

1. 基本类型：在存储时变量中存储的是值本身，占用空间固定，保存在栈中； 如 string ，number，boolean，undefined，null等。
2. 引用类型：存储时变量中存储的是地址，占用空间不固定，保存在堆中； 如 Object、Array、Function等。

### 三、如何判断变量类型

  使用typeof函数可以判断变量类型。

### 四、var let const的区别

  var是函数作用域，let是块作用域，const是块作用域。

### 五、解释一下作用域

​	作用域可以理解为变量起作用的范围，有全局变量和局部变量之分，其次若是在函数中发生嵌套，则按照作用域链的链式规则来进行。

### 六、const定义的值一定不能改变吗

​	const定义的值不能重新修改，也不能重新声明，但是如果const声明的是一个对象，那么是可以修改对象内部的属性的。



