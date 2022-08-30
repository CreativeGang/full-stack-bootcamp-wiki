# Lecture-07 JS: ES6

## 目录

- [Lecture-07 JS: ES6](#lecture-07-js-es6)
  - [目录](#目录)
    - [7.1 Introduce Ecma international](#71-introduce-ecma-international)
      - [7.1.1 版本](#711-版本)
      - [7.1.2 Babel](#712-babel)
    - [7.2 var](#72-var)
      - [7.2.1 Scope](#721-scope)
      - [7.2.2 Re-declare and update](#722-re-declare-and-update)
      - [7.2.3 Hoisting](#723-hoisting)
      - [7.2.4 用 var 的 Problem](#724-用-var-的-problem)
    - [7.3 let](#73-let)
      - [7.3.1 Scope](#731-scope)
      - [7.3.2 Re-declare and update](#732-re-declare-and-update)
      - [7.3.3 Hoisting](#733-hoisting)
    - [7.4 const](#74-const)
      - [7.4.1 Re-declare](#741-re-declare)
      - [7.4.2 Update](#742-update)
    - [7.5 Others](#75-others)
    - [7.6 Conclusion](#76-conclusion)
    - [7.7 Quiz](#77-quiz)
    - [7.8 Template String（模板字符串）](#78-template-string模板字符串)
    - [7.9 Spread operator（展开运算符）](#79-spread-operator展开运算符)
    - [7.10 Destructuring（解构赋值）](#710-destructuring解构赋值)
    - [7.11 Default parameters（默认参数）](#711-default-parameters默认参数)
    - [7.12 Function](#712-function)
      - [7.12.1 High order function](#7121-high-order-function)
      - [7.12.2 Arrow function（老师偏爱，很少写成 nodemon function 的形式）](#7122-arrow-function老师偏爱很少写成-nodemon-function-的形式)
      - [7.12.3 Callback（回调函数）](#7123-callback回调函数)
      - [7.12.4 Callback with arrow function](#7124-callback-with-arrow-function)
      - [7.12.5 Closure（闭包）](#7125-closure闭包)
      - [7.12.6 IIFEs（立即执行函数）](#7126-iifes立即执行函数)
    - [quiz](#quiz)

### 7.1 Introduce Ecma international

#### 7.1.1 版本

- ES5 2009 年
  - 浏览器能够识别的最常用版本
- ES6 2015 年
  - 开发的时候，一般都是用 ES6 及以上的版本。
- ES2022
  - 几乎每年开发一版

#### 7.1.2 Babel

- 解决了开发版本和部署版本不匹配问题。
- 浏览器对 ES5 以外的版本不一定支持，Babel 可以保证新的语法在浏览器上运行，Babel 可以把新的语法转译成 ES5 的语法。

They all used for variable declaration, the main difference is the scope.

### 7.2 var

> 说明：

- var 是 ES5 版本支持的。
- let 和 const 是 ES6 才加入的。
- 我们以后开发用 let 和 const。

#### 7.2.1 Scope

var is function scoped, it means if the variable is declared within a function, it can be accessed within the function. Similarly, if var is used outside of the function, the variable can be accessed in via window object. (in the browser)

> 说明：

- var 属于 function scope。
- 如果声明在一个 function 里面，它就只存在于这个 function；但如果声明在外面，或者说 global 了，它就是一个全局变量。

```js
var apple = 'apple';
function foo() {
  var pear = 'pear';
  console.log(apple); // apple
  console.log(pear); // pear
}
console.log(apple); // apple
console.log(pear); // Uncaught ReferenceError: pear is not defined
```

> 说明：

- apple 可以在 function 里面和外面都访问到。
- pear 只可以在 function 里面被访问到。

#### 7.2.2 Re-declare and update

> 说明：

- var 这个关键字，我们可以用同样的变量名，甚至加上同样的关键字，对这个变量的值进行修改。
- 也可以不加关键字，直接对这个变量的值进行修改。
- 所以它是最宽松的修改方式。可以重新定义，也可以对同一个变量的值进行修改。

```js
var fruit = 'apple';
var fruit = 'pear';
console.log(fruit); // pear
fruit = 'grape';
console.log(fruit); // grape
```

#### 7.2.3 Hoisting

> 说明：

- 我们代码当中的变量在执行的时候，这个变量的声明会被提升到当前它的执行上下文（execution context)的最上面。

```js
console.log(fruit); // undefined
var fruit = 'apple';
```

上面的代码 equals：

```js
var fruit;
console.log(fruit); // undefined
fruit = 'apple';
```

> 说明：

- 变量的 hoisting 用的不太多。不会在声明之前就对他进行使用。
- 只有一种情况下会用到 hoisting，当声明一个函数的时候，函数可能声明在最下面，但是会在上方对这个函数进行调用。我们可以调用这个函数的原因是，函数它也会被变量提升，提到最上面。

#### 7.2.4 用 var 的 Problem

> 说明：

- 在没有 React、Vue 这些框架之前，前端开发需要把多个 Javascript 文件同时导入到一个页面上。当多个文件里有同一个变量名，这些文件被导入到同一个页面上的时候，后一个变量会把前一个变量的值覆盖，从而遇到一些问题。
- 现在不会有这种问题了，因为我们不再使用 var 来变量声明，还有就是我们用模块化的方式来进行开发。
- 模块化下节课会讲到。

```js
var fruit = 'apple';
if (true) {
  // think about this is in another file
  var fruit = 'pear';
}
console.log(fruit); // pear
```

### 7.3 let

#### 7.3.1 Scope

> 说明：

- let 是 block scope。
- block 简单理解为在{}里面的，就是一个 block。
- 特例：const obj = {}; 在 object literal（对象自变量）这样一个自变量里，这个花括号不构成一个 global scope。
- 除此之外，function、if...else...这些的花括号，都能构成一个 block。

for example:
`function() {// this is a block}`
`if(true) {// this is also a block}`
let's reuse the example above but replace the keyword with _let_

```js
let fruit = 'apple';
if (true) {
  let fruit = 'pear';
}
console.log(fruit); // apple
```

#### 7.3.2 Re-declare and update

> 说明：

- 对同一个变量，只能用一次 let。
- 但是可以对这个变量重新赋值。

```js
let fruit = 'apple';
// let fruit = "pear"; // Uncaught SyntaxError: Identifier 'fruit' has already been declared
fruit = 'grape';
console.log(fruit); // grape
```

#### 7.3.3 Hoisting

- let 声明的变量也会有变量提升。
- 与 var 不同，var hoisting 在打印的时候会说 undefined；let hoisting 会抛出一个错误。

```js
console.log(fruit); // Uncaught ReferenceError: Cannot access 'fruit' before initialization
let fruit = 'apple';
```

> 说明：这种错误大家知道就可以。实际开发过程中，不会没声明，就直接对变量进行调用的。

Different to _var_, let is hoisted, but the vairable is not initialized

### 7.4 const

- 和 let 基本上相同。
- 和 let 最大的区别：const 声明的变量，值不可以改变。如果对他重新赋值，js 会报错，说我们尝试对一个常量进行修改。
- 特例：（知识点：我们贮存一个值的时候，是存它的值，还是存它的地址。）
  - 如果我们给声明的常量赋值是一个 primitive type，以为它就不能再被重新赋值。无论怎么赋值，都会报错。
  - 但如果赋的值不是一个 primitive type。是 object 的话，我们不能对它重新赋值（不能给它赋一个全新的 object。因为我们在存 object 的时候，存的是地址，而不是它的实际值）。这时 const 意思是说：不能重新给它指定一个新的地址，只要它是原本的地址，没有问题；所以意味着我们在保证原本地址不变的情况下，是可以对对象里面的值进行修改的。

Scope and the hoisting is the same as _let_

#### 7.4.1 Re-declare

```js
const fruit = 'apple';
// const fruit = "pear"; // Uncaught SyntaxError: Identifier 'fruit' has already been declared
fruit = 'grape'; // Uncaught TypeError: Assignment to constant variable.
console.log(fruit);
```

#### 7.4.2 Update

```js
const fruit = { name: 'apple', color: 'red' };
// fruit = {name: "apple", color: "green"}; // Uncaught TypeError: Assignment to constant variable.
fruit.color = 'green';
console.log(fruit); // {name: "apple", color: "green"}
```

similar in array

```js
const fruits = [];
// fruits = ["apple"]; // Error
fruits.push('apple');
console.log(fruits); // ["apple"]
```

### 7.5 Others

```js
console.log(fruit); // Uncaught ReferenceError: fruit is not defined
```

### 7.6 Conclusion

| keyword | scope    | re-declare | update | hoisting              |
| ------- | -------- | ---------- | ------ | --------------------- |
| var     | function | Yes        | Yes    | Yes (undefined)       |
| let     | block    | No         | Yes    | Yes (not initialized) |
| const   | block    | No         | No     | Yes (not initialized) |

> 说明：

- 三者的 scope 分别是 function、block 和 block；
- var 是可以重新进行声明的，let 和 const 不行；
- var 和 let 可以进行更新，const 不能更新，我们只能对它里面的值进行修改，如果是非 primitive type 的话，可以对它的值进行修改；
- 他们都会有变量提升。只不过他们会得到不同的结果。var 是 undefined；let 和 const 是 not initialized。

Extra reading source
[Var, Let, and Const – What's the Difference?](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)

### 7.7 Quiz

> 先不运行代码，先用脑思考代码结果
> 执行代码最快的方式，是打开浏览器控制台，打开 console

1. What's the output of the following code

```js
var i = 5;
for (var i = 0; i < 3; i++) {
  console.log(i);
}
console.log(i);
```

> 结果：0,1,2,3
> 说明：

- full loop 的终止条件是什么？当 i 的值不满足条件时，本题时 i<3，当 i=3 时，不满足条件，跳出循环。所以此时打印出来的是 3.

2. What's the output of the following code

```js
let i = 5;
for (let i = 0; i < 3; i++) {
  console.log(i);
}
console.log(i);
```

> 结果：0，1，2，5
> 说明：

- 常见错误一：0 的打印没有算进去。i 是从 0 开始的，所以肯定会打印一次 0。
- 常见错误二：有的同学认为会报错。因为我们对同一个变量重新声明。
  - 对同一个变量重新声明的报错，是在同一个作用域下。
  - 但是我们现在出现了两个作用域，一个是 for loop，它自己本身形成了一个 block scope，在这里面重新声明一个变量是没有问题的。
  - 第二个 console.log 处在 for loop 之外，在 global scope 上，i=5.

> 了解：

- 什么是 heap 和 stack？
  - primitive type，会存在 stack 里面；
  - 无序的、不太好控制大小的，放到 heap 里面。

> 说明：

- 当代码从上向下执行的时候，我们有个新的变量 i，i=5.
- 当进入到循环的时候，循环形成了一个 block scope。
- 每个 block 都有一个 execution context（执行栈），当它执行的时候，它里面的值是重新定义的。在这里，i 的值从 0 开始一直往上加。
- 注意：每次循环都是一个 block。所以 i 的值有多个，他们在不同的 scope 里面。他们没有形成覆盖。
- 最后 console.log 的时候，i=5。
- 如果做一个 i=10 的操作，因为用 let 来声明变量，我们可以对变量的值进行覆盖。这个时候 i=10，我们的输出就会从 5 变成 10.所以它是可以被覆盖的。

3.

```js
var arr = [10, 12, 15, 21];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function () {
    console.log('Index: ' + i + ', element: ' + arr[i]);
  }, 1000);
}
```

> 结果：Index: 4, element: undefined
> 说明：就是第 1 题+setTimeout

- 开定时器的目的：为了创造一个异步条件。

  - 我这个代码不是立刻执行的。而是 1s 之后再回来执行的。
  - 关于异步的概念，下节课专门讲，是 JS 一个比较重要且复杂的概念。

- setTimeout 是一个定时器。
- 作用：做一个定时。
- 这里，它接收到两个参数：

  - 第一个参数，传入一个函数。先理解为把一个函数传入了定时器。同时告诉了一个时间。这个时间是以毫秒来算。1000 毫秒=1 秒。
  - 也就是把 1000 毫秒或 1 秒的这样一个概念，传入给了 setTimeout 这样一个函数，它根据我告诉它的时间，在时间到了之后，触发我这个函数。
  - 换句话说就是：当我执行这个代码之后，我告诉引擎，1s 之后，执行我传给你的这个函数。
  - 写这段代码的目的：为了创造一个 1s 的延迟。创造延迟的目的，是为了让 i 最终变成 4.
  - 为什么会变成 4 呢？因为代码在执行的时候，i=0 开始，到 i=4 的时候，跳出循环。同时因为加上一个异步，这个代码不会立刻执行，会在 1s 之后执行。
  - 而 1s 之后循环已经结束了。所以 1s 之后再执行这个函数的时候，执行我传入的这个函数的时候，i=4.

- race condition：比赛谁先跑完。
  - 如果没有这个机制，需要比较谁更快，谁先跑完。
  - 有了异步机制后，我们就可以明确知道，这段代码一定会在之后执行。

> Q&A

1. setTimeout 是什么时候执行的？
2. 循环跳出之后，为什么还会执行 setTimeout？

- A：setTmieout 在循环执行的时候，它就已经执行了；当 i=0 的时候，就执行了 setTmieout。
  function(){}是之后执行的。setTmieout 先执行，setTmieout 内部的内容是之后执行的。

3. 既然停留 1s 执行，为什么还要打印 4 次？

- A：setTmieout 每一次循环都执行，它执行了 4 次。setTmieout 既然执行了 4 次，我们告诉 setTmieout，在它时间到了以后，需要执行的这个函数，也最终会执行 4 次。循环几次，就开启了几次定时器。

4. 为什么前三次的 console.log 没有执行，为什么没有放在 stack 里面，稍后执行？

- A：这也涉及到异步的机制。因为这是异步函数。这个函数的意义在于：它会在 1s 之后才会进入到等待队列里。等待队列的意思是，等待被执行。也就是说，它并没有进入到 stack 里面。这里的 stack 叫做 call stack（调用栈或者执行栈）。

5. 我们的代码是在执行栈里一行行执行的，为什么 function 这段代码不是直接进入到执行栈里面进行执行？

- A：这是一段异步代码。它会在 1s 之后进入到一个等待队列，在等待队列里等待当前调用栈为空的时候，它才会被放到调用栈里面执行。

6. 打印四次，不是每次间隔 1s。

- A：因为在循环的时候，它之间是没有间隔的。可以理解为这次循环调用 setTmieout，倒计时开始，下一次循环马上接着开始执行，然后倒计时又开始，两个倒计时之间是没有间隔的，这也是为什么我们在打印的时候，你可以理解为一次性把四个结果都打印出来。

> 练习：

- 翻牌游戏：如果想要代码，可以管老师要。

### 7.8 Template String（模板字符串）

> 说明：

- ES5 时，没有 Template String（模板字符串），想要把一段话和各种变量放在一起，需要加一堆的"+"，和各种各样的"""，把他们加在一起，通常他们之间有空格，不能忘了，否则会打印出来一个很奇怪的字符串。（如下）
- 有了 Template String（模板字符串）之后，我们可以把他们合并在一起。（如下）
- Template String（模板字符串）关键点在于：`(back tick)，这是启用模板字符串的标志。（如下）
- 在 Template String（模板字符串）里面，如果我们想要表示这是一个变量，会用${}的形式，（如下），${name}, ${age}代表这是一个变量。

Also named, template literal, string interpolation

```js
const name = 'mason';
const age = 104;
// es5
console.log('My name is ' + name + ", and I'm " + age + ' years old');

//es6
console.log(`My name is ${name}, and I'm ${age} years old`);
```

### 7.9 Spread operator（展开运算符）

> 说明：

- 目的：为了让我们做一个快速的**_浅拷贝_**。

  - 比如：我们现在有一个 array，我想创建一个新的 array，同时我想把原来的 array 加上。我们可以把原来的 array 展开，也就是把原来 array 的 1 和 2 展开，放到新的 array 里面，再加上新的 3 和 4，最终得到 1，2，3，4 这也一个 array。（如下）
  - 展开运算符可以对 array 这样使用，但是比较少，一般是对 object 做展开运算符。
  - 对 object 做展开运算符是因为，想要快速的做一个浅拷贝，同时更新拷贝出来的这个对象的某一个属性。
  - 比如我想创建一个新的 fruit，我把原本的 fruit 做一个浅拷贝之后，我再把它的颜色改掉，这样我就得到一个新的 object，newFruit 的名字叫 apple，但是它的颜色已经被改变了。
  - Spread operator（展开运算符）的位置可以放前面，也可以放后面，但是位置不同，对结果有很大的影响。通常 Spread operator（展开运算符）的位置在第一位，因为我们想拷贝完之后，修改它里面的某些值，所以一般放在第一位。
  - 像 newFruit = { color: 'red', ...fruit };，先有一个属性 color，再把 object 展开的话，你会发现和原来的 object 一样，只是位置发生了改变，这也做拷贝的意义就不大。
  - _是否可以理解为后面覆盖前面的？_

- **_浅拷贝_**：（面试当中常见考点）
  - 举例：const fruit = { name: 'apple', color: 'green' };加入一个属性：  
    let newFruit = { name: 'apple', color: 'green'， location: {city: Brisbane}};
  - 做 Spread operator（展开运算符）的操作：  
    let newFruit = { ...fruit, color: 'red' };
  - location 的值是一个 object，所以存的是一个 reference。当我们 spread 的时候，Spread operator（展开运算符）是一个浅拷贝，它只会拷贝最上面的一层。
  - name 和 color 是 primitive type，它会直接拷贝 value。
  - 而 location 是一个 object，它只会拷贝 reference。
  - 如果对 city 做一个修改的话，city: melbourne，fruit 的 location 也会变成 melbourne，这就是做的浅拷贝。

> 建议：

- 了解深拷贝是如何实现的。
- 手写一下深拷贝的代码。

```js
const array = [1, 2];
const newArray = [...array, 3, 4];
console.log(newArray); // [1, 2, 3, 4]
```

```js
const fruit = { name: 'apple', color: 'green' };
let newFruit = { ...fruit, color: 'red' };
console.log(newFruit); // {name: "apple", color: "red"}
newFruit = { color: 'red', ...fruit };
console.log(newFruit); // {color: "green", name: "apple"}
```

### 7.10 Destructuring（解构赋值）

> 说明：

- 目的：在 ES5 的时候，把一些属性从 object 里面取出来，和把属性的值从 object 里面取出来，需要做 Dot Notation（用点的方式，把某一个对象的值取出来）。
- 有了 Destructuring 之后，可以直接写成这样：
  - 等号右边是我们想要解构的对象；
  - 等号左边是我们想要赋的值。
  - 等号左边的格式和右边的值要一一对应。
    比如：
  - 右边是一个 object。想要解构一个 object，一定要在左边加上一对{}，const { name, color } = fruit;
  - {}内的名字一定要一一对应，有 name 和 color 才能写 name 和 color；
  - {}内的顺序没有任何关系。
  - 主要是名字一定要是存在于 object 里面的一个属性，才能够解构出来，否则会输出 undefined

Object destructuring extracts property from object and assigns it to variables.
One way would be using the dot notation

```js
const fruit = { name: 'apple', color: 'red' };
const name = fruit.name; // apple
const color = fruit.color; // red
```

With the new syntax, we don't need to repeatly refer to the `fruit` object

```js
const fruit = { name: 'apple', color: 'red' };
const { name, color } = fruit;
console.log(name); // apple
console.log(color); // red
```

> 说明:

- 实际开发过程中，这个名字已经在上面被声明了，我在下面不能对它重新声明，这时可以把变量改一个名字。
- 改名字的方法通过“：”，（如下）const { name: fruitName, color: fruitColor } = fruit;
- 取 name 属性，赋值的时候，赋到 fruitName 上去。
- 解构的同时做一个命名的变换。

we can also rename the variable

```js
const fruit = { name: 'apple', color: 'red' };
const { name: fruitName, color: fruitColor } = fruit;
console.log(fruitName); // apple
console.log(fruitColor); // red
```

or destructing an array

```js
const fruits = [
  { name: 'apple', color: 'red' },
  { name: 'pear', color: 'green' },
];
const [apple, pear] = fruits;
console.log(apple); // {name: "apple", color: "red"}
console.log(pear); // {name: "pear", color: "green"}
const [{ color: appleColor }, { color: pearColor }] = fruits;
console.log(appleColor); // red
console.log(pearColor); // green
```

> 说明：

- 解构 array 和解构 object 一样的语法。
- 解构 object 时说：“=”右边是什么类型，左边也是要一一对应。
- 解构 array 用[]，有序排列，取值时也要有序，=左右对应。
- 解构 object 同时，也可以把 array 解构出来。

more complicated use cases

```js
const [foo, [[bar], baz]] = [1, [[2], 3]];
console.log(foo); // 1
console.log(bar); // 2
console.log(baz); // 3
```

```js
const [, , third] = ['foo', 'bar', 'baz'];
console.log(third); // "baz"
```

> 注意这个例子：

```js
const [head, ...tail] = [1, 2, 3, 4];
console.log(head); // 1
console.log(tail); // [2, 3, 4]
// Rest element must be last element
```

> 说明：

- 有一个 array，我想把第 0 为 head 解构出来，剩下的用一个展开运算符的标识，然后最后命名为 tail。
- 这样的做法是我把我感兴趣的几位取出来了，剩下的存成一个新的变量 tail，head 是 1，tail 是 array，是剩下的结果。
- tail 的命名方式叫做 rest，展开的部分就是剩下的部分。
- 用展开运算符的时候要注意：rest 一定是在最后一位，只能先取前面的，剩下不要的放 rest 里面。
- 这个解构的方法针对 object 也是一样。把我感兴趣的 property 解构出来，剩下不感兴趣的放在一个 object 里面。

> Q&A:

1. 解构赋值只能用 const 吗？

- A：不是的，可以用 let，但通常情况下用 const。因为取值时，这些值是从外部传进来，已经定义好的，通常情况下不会对这些变量的值进行修改，所以在解构的时候会直接用 const。但是可以用 let。

2. 深拷贝如果想要改变值，是否复制原变量到一个新的变量，然后在新变量上进行修改，可以保持不变？

- A：深拷贝就是多拷贝一层，把所有的层都拷贝一遍，或者说把每一层都做一次浅拷贝，从而达到一个深拷贝的目的。

```js
const [missing = true] = [];
console.log(missing); // true
```

### 7.11 Default parameters（默认参数）

> 说明：

- 当我们在取值的时候，如果某一个变量的值是 undefined，可以给它一个默认值。
- 比如在解构的时候，我想解构某一个变量，如果得到的是 undefined，请把它的值赋值为 true。

```js
function sum(a = 1, b = 1) {
  return a + b;
}
console.log(sum()); // 2
console.log(sum(undefined, 2)); // 3
console.log(sum(3, 4)); // 7
```

> 说明：Function 和 Default parameters

- 我们可以把传入的参数给它一个默认值，function(a,b)，我可以把 a 的值默认设为 1，b 的值默认设为 1。我在调用的时候，我可以不传任何参数，因为它会取默认值。
- 如果想把 a 取默认值，b 取要取的值，传入参数的时候，需要把第一位的值占掉，填的方法就是 undefined，来确保我要传的 2 是对应 b 这个参数上。
- 如果不想要默认参数，在调用的时候传入相应的值就可以了。

> Q&A：

1. 真实工作当中，什么时候会使用默认参数？

- A：很常见，尤其是在解构赋值的时候。有一些 optional 的参数，就是一些配置型的参数，是可以不传的，如果要传的话，就需要一个默认的参数配置，这是函数。
- object 也是一样，我们在传输一个 object 的时候，我只有两个重要的属性，大多数情况下都应是 true 的情况，只要极少数情况下是 false，我可能会把这个参数设置为一个 optional 的参数。
- optional：
  - 在 JS 里，其实不存在 optional 和 compulsory，可以理解为所有的参数都是 optional 的。
  - 在开发的时候，可能会用到 typescript，它对于传入的参数有格式要求，默认情况下，所有的参数都需要有一个类型。而想要指定这个参数不用传，这个参数就会变成 optional 的参数。
  - 这就是当一个参数是 optional 的情况下，我们可能会给它一个默认参数，来省去一些在调用时不必要传入的一些参数。

### 7.12 Function

> 说明：

- 函数在 JS 里叫做 first class objects（JS 里的一等公民），可以理解为函数在 JS 里，就是 object。
- 我们能对它做什么？
  - 1. 赋值。可以把一个 function 赋给一个变量。这也的做法叫做*function expression*。
    - function declaration 和 function expression
      - function declaration：function foo(){}
      - function expression: const foo = function(){}
      - 区别体现在 hoisting 上。
  - 2. 把 function 赋值到某一个对象的属性上。（如下）有 bar 这样一个属性，属性的值是一个 function。
  - 3. 可以把 function 以参数的形式传给另外一个函数。setTimeout 例子。
  - 4. 把函数作为一个函数的返回，给它返回出来，function beta，beta 会返回一个函数，

Functions are first class objects in JS
We can basically treat it like any other javascript objects

```js
// Assign it to a variable
const foo = function (){};

// Or property of a object
const bar = {
  baz: function(){},
  foo: foo,
}

// Pass it as an argument to another function
function alpha(fn) {
  ...
}
alpha(foo);

// Return a function from another function
function beta() {
  return function(){}
}
```

#### 7.12.1 High order function

A “higher-order function” is a function that accepts functions as parameters and/or returns a function.

> 说明：

- 以后学习 react，会知道 high order component（高阶组件），是一个意思。
- High order function 特指一个函数，它接受函数作为参数；或者把一个函数作为返回。
- 这里，alpha、beta 就属于高阶函数。

#### 7.12.2 Arrow function（老师偏爱，很少写成 nodemon function 的形式）

> 说明：

- 是一个语法上的改进。语法上让 function 更简化，尤其是匿名 function。
- 我们之前声明 function 需要关键字，现在可以把他们省略为：
  - 把 function 这个关键字省掉。在()和{}之间加上=>，这个就是 arrow。
  - 在一些特殊情况下，即当只有一行 return 的时候，return 的代码可以写成一行的时候，不管你把它写得多么复杂，只要能写成一行，就可以写成下面的写法：
    - const add = (x, y) => x + y;

```js
const add = function (x, y) {
  return x + y;
};
// vs
const add = (x, y) => {
  return x + y;
};
// equals
const add = (x, y) => x + y;
```

> Q&A:

1. 箭头函数里一定要有 return 吗？

- A：箭头函数里的 return 和普通函数里的 return 是一个意思。

2. 在一个函数里不写 return，什么结果？

- A：如果什么都不写，最终默认返回 undefined。

3. 如果只有一个参数，()是不是可以不写？

- A：可以省略不写，但是不建议。建议几个参数都写().代码规范。

4. 匿名函数是立即执行函数吗？

- A：不是。稍后会讲立即执行函数。它是一个函数，只不过没有名字而已。

5. 只有 return 才可以简写吗？

- A：当你整个函数里面，只有一行 return 的时候才可以简写。

6. 箭头函数没有函数名吗？

- A：箭头函数本身没有函数名，但会给它赋一个变量。

7.

- A：变量可以用 let，只不过函数大概率是不会变他的。所以我们会用 const。绝大多数情况下都用 const。除非想赋给他一个新的 function，用 let 是可以的。

8. 如果一般用 function，关键字还用赋值给 const 的吗？

- A：可以这么写。如果做 function expression 的方式，一般都写成 arrow function 了。如果用 function 关键字，更倾向于写成 function add().

9. add()存的是地址还是值？

- A：function 其实是一个特殊的 object，object 存的是地址，function 也是一样。

10. arrow function 可以直接调用变量名吗？

- A：在创建 arrow 函数的时候，赋了一个变量，在调用的时候，直接执行这个变量加上一个().

11. 例子 1：

```js
const add = (x, y) => {
  const a = 1;
  return a + x + y;
};
```

- 这个时候就不能简写，因为{}内有两行代码，没有办法合并到一行。

12. 例子 2：

```js
const add = (x, y) => {
  if (x > y) {
    return x;
  }
  return y;
};
```

- 可以通过代码的改写变成一行。

```js
const compare = (x, y) => {
  return x > y ? x : y;
  //   if (x>y) {
  //     return x;
  //   }
  //   return y;
};
```

- 简写：

```js
const compare = (x, y) => (x > y ? x : y);
```

13. 关于一行改写：

- A：如果返回是 object 的话，要注意

```js
const returnObj = () =>{
  return { a:1 }
}

改写成：
const returnObj = () => ({a, 1})
```

> 说明：

- 一般{}代表 object 的大括号。但是编译器认为这是函数的括号。所以在改写后，{}外面加一个()，这时上下等同。

14. function 的关键字的简写为函数箭头 function 方式，不需要考虑函数的内容：

- A：绝大多少情况下是的。唯一特例就是涉及到关键字 this 的时候，之后会专门讲 this。

#### 7.12.3 Callback（回调函数）

callback function plays a core role in how js works asynchronously.

Explain in a simple way, pass the function as a parameter and when some _event_ happened (addEventListener), execute this function.

> 说明：

- 重要性尤其体现在异步函数上。
- 把一个函数传给另外一个函数。把传入的函数叫做 callback function。因为这个函数传给另外一个函数之后，可能不是立马执行的，可能是过了一段时间之后再被调用的，或某一件事情触发之后再执行。
- 现实生活中例子：打电话给餐厅订餐，餐厅说晚点打回给你，就是一个 callback。
- 计算器：当用户点击键盘的时候，
  - 首先需要对按键做事件绑定；
  - 绑定好之后，监听事件的触发；
  - 在绑定的时候，就会把想要执行的逻辑传给时间的绑定器；
  - 点击时触发事件，执行绑定好的逻辑，即函数；
  - 这也是一个 callback function。
- 在传入一个 callback function 时，经常会传一个匿名函数，更多的会写成一个 arrow function.

```js
function logger(param) {
  console.log(param);
}
function sum(x, y, callback) {
  setTimeout(function () {
    const total = x + y;
    callback(total);
  }, 1000);
}
sum(1, 2, logger);
```

#### 7.12.4 Callback with arrow function

```js
setTimeout(function () {
  console.log('normal function');
}, 1000);

setTimeout(() => {
  console.log('arrow function');
}, 1000);
```

#### 7.12.5 Closure（闭包）

> 说明：

- 我们想要取值的时候，在当前作用域下取不到，我们要到上一级作用域去查找，这就涉及到闭包。
- lexical scope（词法作用域）：这个作用域是一个静态作用域。我们的代码在写成：

```js
const number = 1;
function foo() {
  console.log(number);
}
```

时，它的作用域就已经决定了。也就是在代码运行的这一刻（固定了），这段代码所形成的作用域就叫做它的“词法作用域”，也就是一个静态的作用域。

- lexical scope（词法作用域）怎么工作的呢？
  - 举例子：有一个函数 foo，在 foo 里，想输出一个 number 的值。注意：在 foo 里是不存在这个变量的，因为在这个函数中没有对它进行声明，clg 它的时候，它其实是到它的上一个作用域去找到这个 number。
  - 又有一个函数 bar，bar 接收一个传参，传参是一个 function，叫做 fn。
  - 机器在读代码的时候：发现这是一个变量，会创建这个变量并且给它赋值；发现这是一个函数，创建一个变量 foo，把一个 function 赋给这个 foo。这个 function 里面干什么，它不关心。只有当它执行的时候，它才关心。
  - 现在我们还没有执行这个 foo，所以对机器来说，它不关心里面是什么东西。
  - 首先明白机器是怎么执行它的。
  - 最后一行调用 bar 这个函数，并且把 foo 传进去。这个时候涉及到执行某个函数了。这个时候回头看 bar。
  - 在 bar 里，创建了一个变量 number，并且赋值为 2。
  - 接着又执行了 function，在执行 function 的时候，需要带入到实际代码段。
  - 整个代码中出现了两个 number，一个是全局的 number，一个是 bar 里的 number。在当前作用域下取不到某个值，会往上作用域查找。foo 的 number 打印出来是 1.
  - function 会接收一系列的 parameters，实际调用的时候，会接收到一系列的 arguments。
  - 把 foo 当作 fn 这个变量传入到 bar 里。
  - 例子 1 的讲解，按照机器的逻辑，连带 execution context、GEC、foo、bar etc.，从 2:12:08 开始，到 2:24:15 结束。
- bar(foo()): 先执行 foo()，把 foo 返回的结果传给 bar。这里 foo 执行是没有 return 的，所以默认返回 undefined，最终 fn 是 undefined，最后对 undefined 进行调用，会抛出错误。

Closures are everywhere in JS. A closure is when a function has access to its lexical scope even when it is called outside of it. Closures are created every time a function is created, at function creation time.

A few complex but common closure cases:

1. a function was passed to another function as param

```js
// we normally don't write this code
const number = 1;
function foo() {
  console.log(number);
}
function bar(fn) {
  const number = 2;
  fn();
}
bar(foo); // 1
```

> 例子 2 说明：

- 例子 1 是把一个函数传给另外一个函数。
- 例子 2 是把一个函数作为返回值返回出来。
- 例子 2 的代码运行过程：
  - 现在有一个 function 叫做 foo，foo 里声明了一个变量 number，在当前 context 下有个变量叫做 number，同时做一个返回，返回了一个函数，函数里打印 number。
  - 函数在取值的时候是通过词法作用域。
  - number 的上一级作用域是 foo function 的 number。
  - 执行的时候，首先调用 foo，foo 会返回一个新的函数。如果打印这行执行的话，在 foo()后面再加上一个()。也就是执行 function foo 得到一个新的 function，再执行这个新的 function。从而执行打印。打印值为 1.

2. a function was returned by another function

```js
function foo() {
  const number = 1;
  return () => {
    console.log(number);
  };
}
const number = 100;
foo()(); // 1
```

We can use this technique to hide some data.

> 说明：

- 闭包的特性，可以用来隐藏一些数据。
- 在 JS 中不存在的一个概念：私有数据。比如声明一个变量，变量的类型是一个函数或 object，把一系列值赋到对象里的某一个属性上。如果才能把某一个值保护起来，让外部访问不到？可以通过 closure。
- 这段函数的目的是想隐藏 counter 这个变量。
- 也就是声明变量在函数里，唯一能够访问这个变量和修改这个变量的方法，封装到函数里面。
  increment 这个函数是给 counter 往上加 1，getCount()是把当前 counter 的值返回出来。然后再把这两个函数导出。这样当我调用 createCounter 的时候，会得到一个对象，这个对象里就会有 increment 和 getCount 这两个属性，分别对两个函数。一个函数把 counter+1，一个可以获取当前数字。但是无论通过任何方法，我都无法访问到这个数字，或者对这个数字进行修改。
- 我在函数里声明一个变量，创建一系列的 function，再把 function 导出，在 function 里面对函数里声明的这个变量进行修改，注意：只有通过函数才能对这些变量进行修改，在外面访问不到函数里面的变量。只能通过返调用这些函数来调用变量值。外部无法直接访问这个变量并对它进行直接的修改。从而达到隐藏这个变量的目的。
- counter 只能通过 getCount()的方式得到当前 count 的值。或者通过调用 increment 这个函数来增加这个变量值。
- 可以避免变量污染。

- 在 JS 里，不存在 class 这个概念。在 ES6 里，加入了 class 关键字，但是只是个语法堂，不存在这个概念。class 本质上就是一个函数。

```js
function createCounter() {
  let counter = 0;
  const increment = () => {
    counter++;
  };
  const getCount = () => {
    return counter;
  };
  return {
    increment,
    getCount,
  };
}
const counter = createCounter();
counter.increment();
console.log(counter.getCount());
```

#### 7.12.6 IIFEs（立即执行函数）

Immediately Invoked Function Expressions
Commonly used to avoid polluting the global namespace and modules!

```js
(function () {
  // some variable in it's own scope
})();
```

> 说明：

- 工作也在当中频繁的使用。
- nodejs 模块化的概念。模块化说白了用的就是 IIFEs，但是 IIFEs 本身用的是 Closure 这样一个技术，来隐藏或避免变量被外部污染。
- 用框架开发的前端，最终在发布的时候，是一个或多个 JS 文件。文件怎么来的？是框架做了一个打包。开发好之后，用命令把项目打包成几个 JS 文件。打包文件的代码会发现很多这样的语法：（如上）。与把变量隐藏起来是一个道理。

### quiz

```js
var scope = 'global scope';
function checkScope() {
  var scope = 'local scope';
  function f() {
    return scope;
  }
  return f();
}
checkScope(); // local
```

```js
var scope = 'global scope';
function checkScope() {
  var scope = 'local scope';
  function f() {
    return scope;
  }
  return f;
}
checkScope()(); // local
```

```js
function createCounter() {
  let counter = 0;
  const increment = () => {
    counter++;
  };
  const getCount = () => {
    return counter;
  };
  return {
    increment,
    getCount,
  };
}
const counter1 = createCounter();
counter1.increment();
console.log(counter1.getCount());

const counter2 = createCounter();
counter2.increment();
counter2.increment();
console.log(counter2.getCount());
```

**_转专业学生如何学习：_**

1. 老师上课的知识点多学几遍，实在听不懂再找别的视频、资料学习。
2. 写一些项目。尽量自己写代码。比如：计算器代码。
3. 计算器写完了，彻底会写了，再找老师要翻牌代码。之后可以联系老师做其他项目。
4. 还有时间的话，预习下节课内容，PPT 老师会提前上传。
5. JS 三大难点：异步、closure、原型链。后端开发用 function programming、前端 react 都有 function，课上只会简单提 class，原型链不会讲（感兴趣自学）。
6. 实战派：老师经历是直接写项目。通过写项目学新的框架，同时提高自己的技术。
7. 学院派：过知识点。
8. promise 可能课堂上没有时间过，属于异步的衍生品，如果讲可能会晚点讲。可以先自学。
9. 前期练手，可以跟着视频打代码做项目。

- 要渐渐脱离跟着视频打。可以尝试停下，自己先做一遍，然后再看视频，跟他对照。
- 老师现在：看别人项目思路，文件结构、数据结构、页面结构，看他怎么设计。

10. postman：桌面应用程序，可以发 http 请求，经常用于测试。有一个路径，看这个路径是否正常返回。
