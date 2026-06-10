---
title: JavaScript
type: javascript
order: 1
---

## let、var、const 的区别

### 声明变量关键字汇总

在 _JavaScript_ 中，一共存在 _3_ 种声明变量的方式：

- _var_
- _let_
- _const_

之所以有 _3_ 种方式，这是由于历史原因造成的。最初声明变量的关键字就是 _var_，但是为了解决作用域的问题，所以后面新增了 _let_ 和 _const_ 的方式。

#### 作用域

首先我们来了解一下作用域。

_ES5_ 中的作用域有：全局作用域、函数作用域，_ES6_ 中新增了块级作用域。块作用域由 { } 包括，_if_ 语句和 _for_ 语句里面的 { } 也属于块作用域。

关于作用域的更多内容，可以参阅《作用域和作用域链》章节。

#### _var_ 关键字

1. 没有块级作用域的概念

```js
//Global Scope
{
    var a = 10;
}
console.log(a); //10
```

上面代码中，在 _Global Scope_（全局作用域）中，且在 _Block Scope_（块级作用域） { } 中，_a_ 输出结果为 _10_，由此可以看出 _var_ 声明的变量不存在 _Block Scope_ 的概念

2. 有全局作用域、函数作用域的概念

```js
//Global Scope
var a = 10;
function checkscope() {
    //Local Scope
    var b = 20;
    console.log(a); //10
    console.log(b); //20
}
checkscope();
console.log(b); //ReferenceError: b is not defined
```

上面代码中，在 _Global Scope_ 中用 _var_ 声明了 _a_，在 _checkscope_ 函数中的 _Local Scope_（本地作用域、函数作用域）中打印出了 _10_，但是在 _Global Scope_ 中打印的变量 _b_ 报错了。

3. 不初始化值默认为 _undefined_

```js
//Global Scope
var a;
console.log(a); //undefined
```

上面代码中，在 _Global Scope_ 中用 _var_ 声明了 _a_，但没有初始化值，它的值默认为 _undefined_，这里是 _undefined_ 是 _undefined_ 类型，而不是字符串。

4. 存在变量提升

```js
//Global Scope
console.log(a); //undefined
var a = 10;

checkscope();
function checkscope() {
    //Local Scope
    console.log(a); //undefined
    var a;
}
```

上面代码中，先打印了 _a_，然后用 _var_ 声明变量 _a_。变量提升是因为 _js_ 需要经历编译和执行阶段。而 _js_ 在编译阶段的时候，会搜集所有的变量声明并且提前声明变量。

可以将这个过程形象地想象成所有的声明（变量）都会被“移动”到各自作用域的最顶端，这个过程被称为提升。

至于 _checkscope_ 函数中的变量 _a_ 为什么输出 _undefined_，可以参阅《作用域和作用域链》章节。

5. 全局作用域用 _var_ 声明的变量会挂载到 _window_ 对象下

```js
//Global Scope
var a = 10;
console.log(a); //10
console.log(window.a); //10
console.log(this.a); //10
```

上面代码中，打印出了 _3_ 个 _10_，访问 _a_ 和 _window.a_ 或是 _this.a_ 都是等价的。

举个例子：比如我要访问 _location_ 对象，使用 _location_ 可以访问，使用 _window.location_ 也可以访问，只不过 _window_ 对象可以省略不写，就像 _new Array( )_ 和 _new window.Array( )_ 是等价的。

6. 同一作用域中允许重复声明

```js
//Global Scope
var a = 10;
var a = 20;
console.log(a); //20

checkscope();
function checkscope() {
    //Local Scope
    var b = 10;
    var b = 20;
    console.log(b); //20
}
```

上面代码中，在 _Global Scope_ 中声明了 _2_ 次 _a_，以最后一次声明有效，打印为 _20_。同理，在 _Local Scope_ 也是一样的。

#### _let_ 关键字

1. 有块级作用域的概念

```js
{
    //Block Scope
    let a = 10;
}
console.log(a); //ReferenceError: a is not defined
```

上面代码中，打印 _a_ 报错，说明存在 _Block Scope_ 的概念。

2. 不存在变量提升

```js
{
    //Block Scope
    console.log(a); //ReferenceError: Cannot access 'a' before initialization
    let a = 10;
}
```

上面代码中，打印 _a_ 报错：无法在初始化之前访问。说明不存在变量提升。

3. 暂时性死区

```js
{
    //Block Scope
    console.log(a); //ReferenceError: Cannot access 'a' before initialization
    let a = 20;
}

if (true) {
    //TDZ开始
    console.log(a); //ReferenceError: Cannot access 'a' before initialization

    let a; //TDZ结束
    console.log(a); //undefined

    a = 123;
    console.log(a); //123
}
```

上面代码中，使用 _let_ 声明的变量 _a_，导致绑定这个块级作用域，所以在 _let_ 声明变量前，打印的变量 _a_ 报错。

这是因为使用 _let/const_ 所声明的变量会存在暂时性死区。

什么叫做暂时性死区域呢？

_ES6_ 标准中对 _let/const_ 声明中的解释 [第13章](https://link.segmentfault.com/?enc=K6pZVwgVNQb0IBQ9LTOuJg%3D%3D.p07UoPCGl5RslJ9ZnW9Nr36NFqs2pU%2FnSfWZUPIH3S1TUXzWdj22pH0lUMFVGVUwJkDpSHrYe8uKlYek%2FK4HBDYkJhc%2Fe2xiWo5V6teR%2BXY%3D)，有如下一段文字：

> _The variables are created when their containing Lexical Environment is instantiated but may not be accessed inany way until the variable’s LexicalBinding is evaluated._

翻译成人话就是：

> 当程序的控制流程在新的作用域（_module、function_ 或 _block_ 作用域）进行实例化时，在此作用域中用 _let/const_ 声明的变量会先在作用域中被创建出来，但因此时还未进行词法绑定，所以是不能被访问的，如果访问就会抛出错误。因此，在这运行流程进入作用域创建变量，到变量可以被访问之间的这一段时间，就称之为暂时死区。

再简单理解就是：

> _ES6_ 规定，_let/const_ 命令会使区块形成封闭的作用域。若在声明之前使用变量，就会报错。
> 总之，在代码块内，使用 _let/const_ 命令声明变量之前，该变量都是不可用的。
> 这在语法上，称为 **“暂时性死区”**（ _temporal dead zone_，简称 **_TDZ_**）。

其实上面不存在变量提升的例子中，其实也是暂时性死区，因为它有暂时性死区的概念，所以它压根就不存在变量提升了。

4. 同一块作用域中不允许重复声明

```js
{
    //Block Scope
    let A;
    var A; //SyntaxError: Identifier 'A' has already been declared
}
{
    //Block Scope
    var A;
    let A; //SyntaxError: Identifier 'A' has already been declared
}
{
    //Block Scope
    let A;
    let A; //SyntaxError: Identifier 'A' has already been declared
}
```

#### _const_ 关键字

1. 必须立即初始化，不能留到以后赋值

```js
// Block Scope
const a; // SyntaxError: Missing initializer in const declaration }
```

上面代码中，用 _const_ 声明的变量 _a_ 没有进行初始化，所以报错。

2. 常量的值不能改变

```js
//Block Scope
{
    const a = 10;
    a = 20; // TypeError: Assignment to constant variable
}
```

上面代码中，用 _const_ 声明了变量 _a_ 且初始化为 _10_，然后试图修改 _a_ 的值，报错。

_const_ 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。

#### 特点总结

- _var_ 关键字

1. 没有块级作用域的概念
2. 有全局作用域、函数作用域的概念
3. 不初始化值默认为 _undefined_
4. 存在变量提升
5. 全局作用域用 _var_ 声明的变量会挂载到 _window_ 对象下
6. 同一作用域中允许重复声明

- _let_ 关键字

1. 有块级作用域的概念
2. 不存在变量提升
3. 暂时性死区
4. 同一块作用域中不允许重复声明

- _const_ 关键字

1. 与 _let_ 特性一样，仅有 _2_ 个差别
2. 区别 1：必须立即初始化，不能留到以后赋值
3. 区别 2：常量的值不能改变

### 真题解答

- _let const var_ 的区别？什么是块级作用域？如何用？

> 参考答案：
>
> 1.  _var_ 定义的变量，没有块的概念，可以跨块访问, 不能跨函数访问，有变量提升。
> 2.  _let_ 定义的变量，只能在块作用域里访问，不能跨块访问，也不能跨函数访问，无变量提升，不可以重复声明。
> 3.  _const_ 用来定义常量，使用时必须初始化(即必须赋值)，只能在块作用域里访问，而且不能修改，无变量提升，不可以重复声明。
>
> 最初在 _JS_ 中作用域有：全局作用域、函数作用域。没有块作用域的概念。
>
> _ES6_ 中新增了块级作用域。块作用域由 { } 包括，_if_ 语句和 _for_ 语句里面的 { } 也属于块作用域。
>
> 在以前没有块作用域的时候，在 _if_ 或者 _for_ 循环中声明的变量会泄露成全局变量，其次就是 { } 中的内层变量可能会覆盖外层变量。块级作用域的出现解决了这些问题。

-_EOF_-

# JS中的数据类型

> 面试题：JS 中的数据类型有哪些？基本类型和引用类型的区别是什么？

- 简单值和复杂值
- 两者之间本质区别
- 两者之间行为区别

## 简单值和复杂值

JS 中的数据类型就分为**两大类**：

- 简单值（基本类型、原始类型）
- 复杂值（引用值、引用类型）

### 1. 简单值

一共**有 7 种**：

- number：数字
- string：字符串
- boolean：布尔值
- undefined：未定义
- null：空
- symbol：符号
- bigint：大数

所谓简单值，是因为**这些类型的值，无法再继续往下拆分**。

**注意点1: symbol 和 bigint**

这两种数据类型从 ES6 开始新增的。

symbol 这种类型主要用于**创建唯一的标识符**。symbol 的值是唯一且不可变的，适用于作为对象属性的键，以及保证不会与其他属性键发生冲突，特别是在多人合作的大型项目中或者当你使用第三方库的时候。

bigint 是一种新增的基本数据类型，它于 ECMAScript 2020（ES11）中被正式添加到语言标准中。bigint 数据类型用于表示大于Number.MAX_SAFE_INTEGER（即 `2^53 - 1`）或小于 Number.MIN_SAFE_INTEGER（即`-2^53 + 1`）的整数。这个类型**提供了一种在 JS 中安全处理非常大的整数的方法**，这在之前的 JS 版本中是不可能的。这种类型非常适合于用在金融、科学计算和加密等领域。

**注意点2: null 和 undefined**

> 面试题1：为什么 null 的数据类型打印出来为 object ？

```js
console.log(typeof null); // object
console.log(typeof undefined); // undefined
```

这其实是 JS 从第一个版本开始时，**设计上的一个遗留问题**。最初的 JS 语言实现是在 1995 年由 Brendan Eich 在 Netscape Navigator 中设计的。在 JS 最初的版本中，**数据类型是使用底层的位模式来标识的，每种数据类型的前几位是用来表示类型信息的**。例如，**对象的类型标记通常以 00 开头**，而由于一个历史错误，**null 被表示为全零（00000000）**，这就使得 null 的类型检查结果与对象一致。

虽然这个行为在技术上是不正确的（因为 null 既不是对象也不包含任何属性），但改变这个行为可能会破坏大量现存的 Web 页面和应用。因此，尽管这是一个众所周知的问题，但由于向后兼容性的考虑，这个设计决策一直未被修改。

不仅没有修改，这个行为目前还被 ECMAScript 标准所采纳，成为了规范的一部分，所有遵循 ECMAScript 标准的 JS 实现都默认在 typeof null 时返回 object.

> 面试题2：为什么 undefined 和 null 明明是两种基础数据类型，但 undefined == null 返回的是 true ？

这个问题其实也是一个历史问题。众所周知，JS 是借鉴了在当时很多已有的语言的一个产物。其中关于“无”这个概念，JS 就是借鉴的 Java，使用 null 来表示“无”的意思，而<u>根据 C 语言的传统，null 被设计成可以自动转为 0</u>.

但是 Brendan Eich 觉得这么做还不够，主要是因为如下两个原因：

1. 由于前面所介绍的设计上的失误，获取 null 的数据类型会得到一个 obect，这在开发上会带来一些未知的问题。
2. JS 在设计之初就是弱类型语言，当发生数据类型不匹配的时候，往往会自动数据类型转换或者静默失败，null 自动转为 0 的话也很不容易发现错误。

基于上面的这些理由，Brendan Eich 又设计出来了 undefined. 也就是说，undefined 实际上是为了填补 null 所带来的坑。

```js
console.log(null + 1); // 1
console.log(undefined + 1); // NaN
```

目前来讲，关于 null 和 undefined 主要区别总结如下：

- null：从语义上来讲就是表示对象的 “无”
    - 转为数值时会被转换为 0
    - 作为原型链的终点
- undefined：从语义上来讲就是表示简单值的“无”
    - 转为数值为 NaN
    - 变量声明了没有赋值，那么默认值为 undefined.
    - 调用函数没有提供要求的参数，那么该参数就是 undefined
    - 函数没有返回值的时候，默认返回 undefined.

### 2. 复杂值

**复杂值就一种**：object

之所以被称之为复杂值，就是**因为这种类型的值可以继续往下拆分，分为多个简单值或者复杂值**。

```js
const obj = {
    name: '张三',
    age: 18,
    scores: {
        'htmlScore': 99,
        'cssScore': 95,
    },
};
```

**像数组、函数、正则这些统统都是对象类型，属于复杂值**

```js
console.log(typeof []); // object
console.log(typeof function () {}); // function
console.log(typeof /abc/); // object
```

**函数的本质也是对象。**

```js
function func() {}
// 该函数我是可以正常添加属性和方法的
func.a = 1; // 添加了一个属性
func.test = function () {
    console.log('this is a test function');
}; // 添加了一个方法
console.log(func.a); // 1
func.test(); // this is a test function
```

在函数内部有一个特别的内部属性 `[[Call]]`，这个是属于内部代码，开发者层面是没有办法调用的。但是**有了这个属性之后，表示这个对象是可以被调用。**

因为函数是可调用的对象，为了区分 **普通对象** 和 **函数对象**，因此当我们使用 typeof 操作符检测一个函数时，它返回的是 function。

也正因为这种设计，所以 JS 中能够实现高阶函数。高阶函数的定义：

- 接受一个或多个函数作为输入
- 输出一个函数

因为在 JS 中，函数的本质就是对象，因此可以像其他普通对象一样，作为参数或者返回值进行传递。这也是 JS 中所说的函数是一等公民这个说法的由来。

## 两者之间本质区别

介绍完了简单值和复杂值之后，接下来我们从内存存储的角度，来看一下这两种本质上的区别。

我们知道，内存的存储区域可以分为 **栈** 和 **堆** 这两大块。

- 栈内存：**栈内存因为其数据大小和生命周期的可预测性而易于管理和快速访问**。栈支持快速的数据分配和销毁过程，**但它不适合复杂的或大规模的数据结构**。
- 堆内存：**堆内存更加灵活，可以动态地分配和释放空间，适合存储生命周期长或大小不确定的数据**。使用堆内存可以有效地管理大量的数据，但相对于栈来说，其管理成本更高，访问速度也较慢。

对于**简单值而言，它们通常存储在栈内存里面**。上面说了，栈内存的特点是管理简单且访问速度快，适用于存储 **大小固定、生命周期短** 的数据。简单值的存储通常包括直接在栈内存中分配的数据空间，并且直接存储了数据的实际值。

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2024-04-30-031914.png" alt="image-20240430111914109" style="zoom:50%;" />

而对于复杂值而言，具体的值是存储在 **堆内存** 里面的。因为**复杂值往往大小是不固定的，无法在栈区分配一个固定大小的内存**，因此**具体的数据放在堆里面**。那么这就没有栈区什么事儿了么？倒也不是，**栈区会存储一个内存地址**，通过该内存地址可以访问到堆区里面具体的数据。

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2024-04-30-032917.png" alt="image-20240430112917412" style="zoom:50%;" />

另外讲到这里，还有一个非常重要的点要提一下，那就是 **JS 中在调用函数的时候，通通都是值传递，而非引用传递**。

```js
function test(obj) {
    obj.a = 1000;
}
const obj = {};
console.log(obj); // {}
test(obj);
console.log(obj); // { a: 1000 }
```

上面的代码，有一定的迷惑性。你看到上面的代码，觉得调用函数之后，obj 发生了真实的修改，所以这是一个引用传递。

但是这里**仍然是一个值传递**。只不过这个值的背后对应的是一个地址值，这个地址值和简单值一模一样，会被复制一份传递给函数，然后函数内部拿到的是地址值，就可以通过这个地址值找到同一份堆区数据。

```js
function test(obj) {
    obj = { b: 1 }; // 这里就赋值了一个新对象，不再使用原来的对象
    obj.a = 1000;
}
const obj = {};
console.log(obj); // {}
test(obj);
console.log(obj); // {}
```

如果是真正的引用传递，那么函数内部的 obj 和外部的 obj 是绑在一起的，函数内部对 obj 做任何修改，都会影响外部。但是上面的代码中，很明显在函数内部对 obj 重新赋值后，断开了内外的联系，因此**在 JS 中只有值传递**。

## 两者之间行为区别

聊完了本质区别后，接下来我们再来聊一下两者之间行为的区别，主要就下面这么几个点：

1. 访问方式
2. 比较方式
3. 动态属性
4. 变量赋值

### 1. 访问方式

简单值是 **按值访问**，也就是说，一个变量如果存储的是一个简单值，当访问这个变量的时候，得到就是对应的值。

```js
const str = 'Hello';
console.log(str);
```

复杂值是虽然也是 **按值访问** ，但是由于值对应的是一个 **内存地址值**，一般不能够直接使用，还需要进一步获取地址值背后对应的值。

```js
const obj = { name: '张三' };
console.log(obj.name);
```

### 2. 比较方式

这个比较重要，**无论是简单值也好，复杂值也好，都是进行的值比较**。不过由于复杂值对应的值是一个 **内存地址值**，因此只有在这个内存地址值相同时，才会被认为是相等。

```js
const a = {}; // 内存地址不一样，假设 0x0012ff7c
const b = {}; // 内存地址不一样，假设 0x0012ff7d
console.log(a === b); // false
```

### 3. 动态属性

对于**复杂值来讲，可以动态的为其添加属性和方法，这一点简单值是做不到的**。

如果为简单值动态添加属性，不会报错，会静默失败，访问时返回的值为 undefined

但如果为简单值动态添加方法，则会报错 xxx is not a function.

```js
const a = 1;
a.b = 2;
console.log(a.b); // undefined
a.c = function () {};
a.c(); // error
```

### 4. 变量赋值

最后说一下关于赋值，记住，它们都是 **将值复制一份** 然后赋值给另外一个变量。

不过由于复杂值复制的是 **内存地址**，因此修改新的变量会对旧的变量有影响。

```js
let a = 5;
let b = a;
b = 10; // 不影响 a
console.log(a);
console.log(b);
let obj = {};
let obj2 = obj;
obj2.name = '张三'; // 会影响 obj
console.log(obj); // { name: '张三' }
console.log(obj2); // { name: '张三' }
obj2 = { name: '张三' };
obj2.age = 18; // 不会影响 obj
console.log(obj);
console.log(obj2);
```

---

-EOF-
