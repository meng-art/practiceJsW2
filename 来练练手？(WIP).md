## 来练练手？  (WIP)

### 简答

#### 一、JS部分

##### 1.JavaScript 有哪些数据类型？

八种数据类型
Number，BigInt，String，Boolean，Undefined，Null，Symbol，Object

##### 2.Symbol 和 Bigint 简单介绍

- symbol：符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。
- bigint：表示任意大的整数。

##### 3.说一说基本类型和引用类型的区别

- 基本类型：在存储时变量中存储的是值本身，占用空间固定，保存在栈中； 如 string ，number，boolean，undefined，null等。
- 引用类型：存储时变量中存储的是地址，占用空间不固定，保存在堆中； 如 Object、Array、Function等。

##### 4.var let const 的区别

var是函数作用域，let是块作用域，const是块作用域，且var具有变量提升的性质var可以跨块访问，但是不能跨函数访问。

##### 5.如何判断变量类型

可以使用Object.prototype.toString.call（）

```javascript
    Object.prototype.toString.call(new Date()) // [object Date]
    Object.prototype.toString.call("1") // [object String]
    Object.prototype.toString.call(1) // [object Numer]
    Object.prototype.toString.call(undefined) // [object Undefined]
    Object.prototype.toString.call(null) // [object Null]
```

##### 6.如何判断一个变量是null？

```javascript
Object.prototype.toString.call(null) // [object Null]
```

##### 6.undefined 和 null 的区别

- undefined表示未定义的值，这种状态会在下面四种情况出现：

  ```
  //1.声明一个变量，但没有赋值
  var foo;
  console.log(foo); // undefined
  
  //2.访问对象上不存在的属性或者未定义的变量
  console.log(Object.foo); // undefined
  console.log(typeof demo); // undefined
  
  //3.函数定义了形参，但没有传递实参
  function fn(a) {
      console.log(a); // undefined
  }
  fn(); //undefined
  ```

-   null：表示“空值”，代表一个空对象指针，使用 typeof 运算得到 “object”。一般会在两种情况下给变量赋值为null

  <ol type="i">
      <li>如果定义的变量在将来用于保存对象，那么最好将该变量初始化为null</li>
      <li>当一个数据不再需要使用时，我们最好通过将其值设置为null来释放其引用，这个做法叫做解除引用</li>
  </ol>

##### 7.Const 定义的值一定不能改变吗？

  	  const 定义的基本类型不能改变，但是定义的对象是可以通过修改对象属性等方法改变的。

##### 8.const 声明生成对象的时候，如何使其不可更改？

​        obeject.freeze():冻结一个对象，被冻结对象自身的所有属性都不可能以任何方式被修改，新增属性。但是如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象。那若使对象的属性或值不能改变，可以通过freeze()封装的深冻结函数使对象为静态的。

```javascript
// 深冻结函数.
function deepFreeze(obj) {

  // 取回定义在obj上的属性名
  var propNames = Object.getOwnPropertyNames(obj);

  // 在冻结自身之前冻结属性
  propNames.forEach(function(name) {
    var prop = obj[name];

    // 如果prop是个对象，冻结它
    if (typeof prop == 'object' && prop !== null)
      deepFreeze(prop);
  });

  // 冻结自身(no-op if already frozen)
  return Object.freeze(obj);
}
```

##### 9.闭包的应用，用来解决什么问题，谈谈理解

​          闭包 = 内层函数+外层变量，他可以使外部函数访问函数内部的变量，实现数据的私有。 

```javascript
//1.外部函数访问函数内部的变量
function outer() {
      const a = 1
      function fn() {
        console.log(a)
      }
      return fn
    }
const fun = outer();
fun() //1

//outer() == fn == function fn(){}

//2.实现数据的私有
function count() {
    let i = 0;
    function fn() {
        i++;
        console.log(`函数调用了${i}次`);
    }
    return fn;
}
const result = count();
i = 1000;//这里的i将不起作用
result() //函数调用了1次
result() //函数调用了2次
result() //函数调用了3次
```

##### 10.说一说箭头函数中 this 的指向

&emsp;&emsp;箭头函数没有prototype，所以箭头函数本身没有this，箭头函数的this只会从自己作用域链的上一层继承this，在箭头函数定义时他会捕获自己所处的执行上下文环境的this并继承下来，所以箭头函数的this是在他被定义的时候就已经确定了，之后不会再改变了。

- **全局执行上下文 在浏览器中全局上下文的变量对象就是window对象**

  ```javascript
  // 在浏览器控制台直接console this
  console.log(this)
  // window对象
  ```

- **函数指向上下文**

  ```javascript
  const obj = {
    a: 1,
    b: () => {
      console.log(this.a)
    }
  }
  obj.b() // undefined
  ```

  &emsp;&emsp;这里的解释是obj对象没有执行上下文环境，所以向上查找到全局执行上下文环境，即window，window.a为undefined。

  ```javascript
  const obj1 = {
    a: 1,
    b: function () {
      return () => {
        console.log(this.a)
      }
    }
  }
  obj1.b()() // 1
  ```

  &emsp;&emsp;这里就是箭头处于了一个函数执行上下文环境b，而b的this指向obj1，所以这里打印出来是个1。

##### 11.箭头函数可以用 call 或者 apply 改变 this 指向吗？

​      箭头函数中的 this 在定义它的时候已经决定了，与如何调用以及在哪里调用它无关，包括 (call, apply, bind) 等操作都无法改变它的 this。

##### 12.bind、apply、call 的区别

-   call(),apply(),bind()都是用来重定义this这个对象的。

- call，bind，apply这三个函数的第一个参数都是this的指向对象，直到第二个参数差别就出来了。

- call的参数是直接放进去的，直接用逗号分离，apply的所有参数都必须放在一个数组里面传进去，除了**bind返回是函数**以外，它的参数和call的放入方式一样。

  ```javascript
        let obj = {
          name: this.name,
          objAge: this.age,
          myLove: function (fm, t) {
            console.log(this.name + "年龄" + this.age, "来自" + fm + "去往" + t);
          },
        };
        let obj1 = {
          name: "huang",
          age: 22,
        };
        obj.myLove.call(obj1, "达州", "成都"); //huang年龄22来自达州去往成都
        obj.myLove.apply(obj1, ["达州", "成都"]); //huang年龄22来自达州去往成都
        obj.myLove.bind(obj1, "达州", "成都")(); // huang年龄22来自达州去往成都，
  //注意：由于bind返回的是一个函数，所以调用时后面别忘记加()
  ```

##### 13.说说 new 都做了些什么

- 创建一个新的对象`obj`

- 将对象与构建函数通过原型链连接起来

- 将构建函数中的`this`绑定到新建的对象`obj`上

- 根据构建函数返回类型作判断，如果是原始值则被忽略，如果是返回对象，需要正常处理。

  

#### 二、CSS部分

##### 1.说说 position

- static：无定位的意思。

- relative：元素在移动位置的时候，是相对于它自己**原来的位置**来说的，**相对定位并没有脱标**，它最典型的应用是给绝对定位当爹的。
- absolute：元素在移动位置的时候，是相对于它**祖先元素**来说的，**绝对定位完全脱标**，**父元素没有定位**，则以**浏览器**为准定位。
- fixed：元素**固定于浏览器可视区的位置**，浏览器的可视窗口为参照点移动元素，**固定定位完全脱标**。
- sticky：相对定位和固定定位的混合，**粘性定位不脱标**（不常用）。

| 定位模式 | 是否脱标         | 移动位置           |
| -------- | ---------------- | ------------------ |
| static   | 否               | 不能使用边偏移     |
| relative | 否（占有位置）   | 相对于自身位置移动 |
| absolute | 是（不占有位置） | 带有定位的父级     |
| fixed    | 是（不占有位置） | 浏览器可视区       |
| sticky   | 否（占有位置）   | 浏览器可视区       |

##### 2.flex 属性都有什么，其作用是什么

2.1 容器属性

- flex-direction：属性决定主轴的方向（默认是x轴）。

- flex-wrap：默认的情况下，Flex容器中的子元素都排在一行，如果子元素的总宽度大于父元素的宽度，子元素就会被压缩。`flex-wrap`属性可以设置子元素换行。

- justify-content：定义子元素在主轴上的对齐方式，它主要有5种取值：

  ```
  flex-start: 默认值，从起点开始
  flex-end: 从终点对齐
  cneter: 居中对齐
  space-between: 两端对齐，子元素之间的间隔相等
  space-around: 每个子元素的左右间隔相等
  ```

- align-items：定义子元素在侧轴上的对齐方式==（单行下有效）==。

- align-content：定义子元素多根轴线在侧轴上的对齐方式，==只在多行显示下有效==。

- flex-flow：flex-direction和flex-wrap的组合，它是将这两个属性写到一起，先写flex-direction，后写flex-wrap。

2.2 子元素属性

- align-self：==允许单个项目==有与其他项目不一样的对齐方式，可覆盖 align-items 属性。

- order：定义项目的排列顺序，数值越小，排列越靠前，默认为0。       

  

##### 3.CSS 优先级

| 选择器               | 选择器权重 |
| -------------------- | ---------- |
| 继承或者*            | 0,0,0,0    |
| 元素选择器           | 0,0,0,1    |
| 类选择器，伪类选择器 | 0,0,1,0    |
| ID选择器             | 0,1,0,0    |
| 行内样式style=""     | 1,0,0,0    |
| !important           | 无穷大     |

选择器权重可以叠加，但要遵循一下几点规则：

- 权重是有4组数字组成,但是不会有进位，可以理解为类选择器永远大于元素选择器, id选择器永远大于类选择器。
- 等级判断从左向右，如果某一位数值相同，则判断下一位数值。

##### 4.元素水平垂直居中

1. 行内元素：

   - 水平居中：给父级元素使用属性`text-align：center`

   - 垂直居中：给父级元素使用属性`line-hight：父元素盒子的高度`

2.  块级元素：

   - flex布局：

     ```css
     display: flex;
     justify-content: center;
     align-items: center;
     ```

   - position：absolute：

     ```
     position: absolute;
     left: 50%;
     top: 50%;
     transform: translate(-50%, -50%);
     ```

     

#### 三、异步

##### 1.说说Event Loop

 JS中先执行同步操作，再执行微任务（promise里的then（）等），最后执行宏任务（settimeout等）

##### 2.EventLoop JS 事件循环队列、宏任务和微任务

存放异步事件的地方，叫做事件队列，异步任务分为宏任务和微任务，并且先执行微任务，再执行宏任务。

##### 3.promise都有哪些方法

Promise 的实例方法有 then/catch/finally 三种，静态方法有 all/race/allSettled/any/resolve/reject 六种。实例方法只有在promise实例化后才能使用。

#### 四、react

##### 1.Hooks用过吗？聊聊React中class组件和函数组件的区别？

1. class组件有状态,函数组件中不能拥有自己的状态(state)。在hooks之前函数组件是无状态的，都是通过props来获取父组件的状态，但是hooks提供了useState来维护函数组件内部的状态。
2. class组件有生命周期，并且较为复杂，函数组件没有生命周期。

##### 2.介绍一下React 生命周期

生命周期分为四个阶段：`挂载阶段`，`更新阶段`，`卸载阶段`，`错误处理`。



### 看程序说结果

##### 1.类型判断

```javascript
console.log(typeof typeof typeof null)
// string
```

因为typeof null是'object'，这是一个string。

##### 2.作用域

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i); 
  }, 1)
}
//因为var是全局作用域，所以输出3,3,3

for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1)
}
//let是块级作用域，所以输出是0,1,2
```

```javascript
function sayHi() {
    console.log(name);  //var具有变量提升的作用，但是只提升声明，不赋值，所以是undefined
    console.log(age);   //let不具有变量提升作用，报错。
    var name = 'Lyric';
    let age = 19;
}
sayHi();
```

```javascript
function fn(a) {
  console.log(a);  //ƒ a() { }  因为函数提升优先级高于变量提升
  var a = 2;
  function a() {}
  console.log(a);  //2
}
 
fn(1);
```

```javascript
if ('a' in window) {
    var a = 10;
}
alert(a); //10         （不懂）
```

```javascript
function f(x){
    var x;
    console.log(x);
}

f(5);  //5
```

```javascript
var a;
a = 5;
var a;

console.log(a); // 5 这里和参数合并成一个了
```

```javascript
var a = 1;
function f() {
console.log(a);  //undefined var变量提升的原因
var a = 2;
}
f()  
```

```javascript
var a = 10;
a.pro = 10;
console.log(a.pro + a);              //（不懂）
 
var s = 'hello';
s.pro = 'world';
console.log(s.pro + s);
```

```javascript
var a = 100;
function fn() {
  var b = 30;
  function bar() {
      console.log(a + b);  // 130
      console.log(this.b); //400  （不是很懂，是因为function fn（）不能作为对象嘛）
  }
  return bar;
}
var x = fn(),
b = 400;
x();


var a = 100;
var b = 30;
function bar() {
    console.log(a + b);  //130
    b=400;
    console.log(this.b);  //400
}
bar()
```

```javascript
var f = true;
if (f === true) {
  var a = 10; 
}
 
function fn() {
  var b = 20; 
  c = 30; 
}
 
fn();
console.log(a);//10 全局变量
console.log(b);// Uncaught ReferenceError: b is not defined
console.log(c);//30 未加var等关键字命名的变量，当作全局变量
```

