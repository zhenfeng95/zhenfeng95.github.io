---
title: TypeScript介绍
type: ts
order: 4
---

## 类型理解

### any

在TS中，编译时一切都要有类型，如果你和TS类型检查器无法确定类型是什么，默认为`any`。这是兜底的类型，是TS中所有类型的教父。

```javascript
let a: any = 666;
let b: any = ['danger'];
let c = a + b;
```

正常情况下，第三个语句应该在TS中报错才对（谁会去计算一个数字和一个数组之和呢？）

但是如果显示声明了any标注，就不会报错，其实这里的做法就和原生JS的处理一模一样了。

换句话说，如果要使用any，一定要显示标注，如果TS推导出值的类型为any（例如忘记注解函数的参数，或者引入没有类型的JavaScript模块），将抛出运行时异常。

```javascript
let foo; // any

function func(foo, bar) {} // error 参数"foo","bar"隐式具有“any”类型。
```

> 默认情况下，Typescript是宽容的，在推导出类型为any时其实不会报错，如果在`tsconfig.json`中启用了`noImplcitAny`标志，就会遇到隐式any类型时报错。
>
> `noImplcitAny`隶属于TSC的`strict`标志家族，如果已经在`tsconfig.json`中启用了`strict`，那就不需要专门设置`noImplcitAny`标志了，效果是一样的。

有时候我们可能确实需要一个表示任意类型的变量，特别是从javascript代码移植到typescript的时候。比较明显的比如`console.log()`方法就能接收任意类型的参数。

当然默认情况下，你看到的应该是这样的

```javascript
 log(...data: any[]): void;
```

我们现在能看到类型提示，这是由于VS Code编辑器结合着`lib.dom.d.ts`文件提供的TS支持。

如果已经安装了`@types/node`，可以得到nodejs对于`console.log`函数更加细致的提示：

```javascript
log(message?: any, ...optionalParams: any[]): void;
```

> 关于@types的内容，我们在快速入门中已经讲过。
>
> Node.js 的核心模块和某些第三方模块并不是天然支持Typescript的。这就意味着，如果在 TypeScript 项目中使用这些模块时，编译器无法得知这些模块的类型信息，从而无法提供类型检查和自动补全的功能。比如下面的代码会报错：
>
> ```typescript
> const fs = require('fs'); // error 找不到名称require,需要Nodejs类型定义
> ```
>
> 我们可以手动安装nodejs的TypeScript 社区[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) 提供的声明文件库。当使用 TypeScript 开发 Node.js 项目时，`@types/node` 库可以为 Node.js 的核心模块和常用的第三方模块提供类型定义，以便在开发过程中获得类型检查和自动补全的支持。
>
> ```javascript
> npm i @types/node -D
> ```
>
> 这样上面代码`const fs = require('fs');`也找到的对应的类型支持，在TS文件中不会再报错了。

总的来说，你可以在 any 类型变量上任意地进行操作，包括赋值、访问、方法调用等等，此时可以认为类型推导与检查是被完全禁用的：

```javascript
let anyVar: any = null;
anyVar.foo.bar.fn();
anyVar[0][1][2].prop;
```

正如我们一开始就强调的**【any兜底的类型，是TS中所有类型的教父】**

> **any能兼容所有类型，也能够被所有类型兼容**

这一作用其实也意味着类型世界给你开了一个外挂，无论什么时候，你都可以使用 any 类型跳过类型检查。当然，运行时出了问题就需要你自己负责了。

any 类型的万能性也导致我们经常滥用它，比如类型不兼容了就 any 一下，类型不想写了也 any 一下，不确定可能会是啥类型还是 any 一下。此时的 `TypeScript` 就变成了令人诟病的 `AnyScript`。

### unknown

> 少数情况下，如果确实无法预知一个值的类型，不要使用any，更合理的方式是使用 unknown

unknown也表示任何值，一个 unknown 类型的变量可以再次赋值为任意其它类型，但只能赋值给 any 与 unknown 类型的变量

```javascript
let a: unknown = 30;
let b = a === 30;

let c: any = 30;
let d:number = c + 10;

let e: unknown = "string";
e = 123;
let f: any = e;
// let f:string = e; // error 不能将类型unknown分配给类型string
//let f = e + 10; //error "e"的类型为"未知"
if(typeof e === "number") {
  let g = e + 10;
}
```

1. TS不会把任何值推导为`unknown`类型，必须显式注解
2. `unknown`类型的值可以比较
3. `unknown`类型的变量可以赋值给`any`或者`unknown`类型的其他变量
4. 但是执行操作时不能假定`unknown`类型的值为某种特定的类型（比如上面的运算，注意和any的区别），必须先向TS证明一个值确实是某个类型，可以使用typeof

简单的说，**any 放弃了所有的类型检查，而 unknown 并没有**。

```javascript
let anyFn:any;
let unknownFn: unknown;

anyFn.foo();
unknownFn.foo(); // error 对象的类型为"unknown"
```

**在类型未知的情况下，更推荐使用 unknown 标注。**这相当于你使用额外的心智负担保证了类型在各处的结构，后续重构为具体类型时也可以获得最初始的类型信息，同时还保证了类型检查的存在。当然，unknown 用起来很麻烦。

......如果本身就出现了不得不使用any或者unknown的情况，没必要太过于纠结使用any还是unknown，归根结底，用哪个完全取决于你自己，毕竟语言只是工具

### boolean与类型字面量

`number`,`boolean`,`string`,`symbol`,`bigint`这些js本身就支持的基础类型使用起来很简单，ts的书写几乎感觉不到和js的差别，而且支持很多种书写的方式，当然中间还隐藏着一些很重要的细节。拿boolean举例来说：

```javascript
let a = true;
var b = false;
const c = true;
let d: boolean = true;
let e: true = true;
let f: false = false;
//let g: true = false; // error 不能将类型false分配给类型true
```

1. **可以让TS推导出值的类型为boolean（a，b）**
2. **可以明确的告诉TS，值的类型为boolean（d）**
3. **可以明确的告诉TS，值为某个具体的boolean值（e，f和g）**
4. **可以让TS推导出(const)值为某个具体的布尔值（c）**

首先我们常见的写法是1-4（行），要么使用TS自己的类型推导，要么我们自己定义好boolean类型，这是我们开始就介绍的方式。但是，5-7（行）的写法是什么意思？

其实写法也很直观，我们大概也能猜到，**变量e和f不是普通的boolean类型，而是值只为true和false的boolean类型**

> 把类型设为某个值，就限制了e和f在所有布尔值中只能取指定的那个值。这个特性称为类型字面量（type literal）
>
> **类型字面量：仅仅表示一个值的类型**

由于类型字面已经限定了具体的类型true或者false，因此上面代码第7行的错误就可以理解了：

```javascript
let g: true = false; // error 不能将类型false分配给类型true
```

特别注意一下第三行的代码：`const c = true;`，这里的变量c的类型是类型字面量true。

> 因为const声明的基本类型的值，赋值之后便无法修改，因此TS推导出的是范围最窄的类型

### number与bigint

有了上面boolean类型的说明，其他的基本数据类型基本一致

> bigint是ES11(ES2020)新增的一种基本数据类型，在JS中，可以用 Number 表示的最大整数为 2^53 - 1，可以写为 Number.MAX_SAFE_INTEGER。如果超过了这个界限，那么就可以用 BigInt 来表示，它可以表示任意大的整数。
>
> 在一个整数字面量后面加 n 的方式定义一个 bigint，或者调用函数 BigInt()
>
> **注意这里强调的问题：ES11（ES2020），如果编译的时候没有指定tsconfig的target（指定代码编译成的版本）和lib（TSC假定运行代码的环境）为es2020以上的版本，或者执行tsc的时候，没有指定--target为es2020以上版本，将会编译报错**

```javascript
let a = 123;
let b = Infinity * 0.10;
const c = 567;
let d = a < b;
let e: number = 100;
let f: 26.218 = 26.218;
// let g: 26.218 = 10; // error 不能将类型10分配给类型26.218

let a1 = 1234n;
const b1 = BigInt(1234);
const b2 = 1234n;;
let d1 = a < a1;
// let e1 = 1234.5n; // error bigint字面量必须是整数
// let f1: bigint = 1234; // error 不能将类型number分配给类型bigint
let g1: bigint = 100n;
let h1: 100n = 100n;
```

1. **可以让TS推导出值的类型为number/bigint（a，b，a1，b1）**
2. **可以明确的告诉TS，值的类型为number/bigint（e，f1）**
3. **可以明确的告诉TS，值为某个具体的number/bigint值（e，f，g，g1，h1）**
4. **可以让TS推导出(const)值为某个具体的number/bigint值（c，b2）**

### string

与boolean和number形式是一样的，而且string字符串形式同样有单引号`''`,双引号`""`和模板字符串``的形式

> 模板字符串还可以有其他的作用，这个在后期再给大家介绍
