### [let和const命令](http://es6.ruanyifeng.com/#docs/let)

### let特性 
- 不存在变量提升
- 暂时性死区
- 不允许重复声明

### 块级作用域
- 块级作用域与函数声明

ES6规定，块级作用域之中，函数声明语句的行为类似于let，在会计作用域之外不可引用
ES6在附录B里面规定，浏览器的实现可以不遵守上面的规定，有自己的行为方式
	- 允许在块级作用域内声明函数
	- 函数声明类似var，会提升到全局作用域或函数作用域的头部
	- 同事，函数声明还是会提升到所在的会计作用域的头部
上面三条规则只对ES6的浏览器实现有效，其他环境的实现不用遵守

### do表达式

### const
- const声明一个只读的常量，一旦声明，常量的值就不能改变
- 对于const来说，只声明不赋值，就会报错
- const的作用域与let命令相同：只在声明所在的块级作用域内有效
- const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用
- const声明的变量，与let一样不可重复声明

#### 本质
const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动

### ES6声明变量的六种方法
- var (ES5)
- function (ES5)
- let 
- const
- import
- class

### 顶层对象的属性
顶层对象，在浏览器环境指的是window对象，在Node指的是global对象，ES5中，顶层对象的属性与全局变量是等价的

ES6改变这一点，为了保持兼容性，var命令与function命令声明的全局变量依旧为顶层对象的属性
let命令，const命令，class命令声明的全局变量，不属于顶层对象的属性

### global对象
ES5的顶层对象本身也是一个问题，因为它在各种实现里面是不统一的

- 浏览器里面，顶层对象是window，但Node和Web Work没有window
- 浏览器和Web Worker里面，self也指向顶层对象，但Node没有self
- Node里面，顶层对象是global，但其他环境都不支持

同一段代码为了能够在各种环境，都能渠道顶层对象，现在一般是使用this变量，但是有局限性

- 全局环境中，this会返回顶层对象，但是，Node模块和ES6模块中，this返回的是当前模块
- 函数里面的this，如果函数不是作为对象的方法运行，而是单纯作为函数运行，this会指向顶层对象，但是严格模式下，this会返回undefined
- 不管是严格模式还是普通模式，new Function('return this')(),总是会返回全局对象。但是如果浏览器用来CSP（Content Security Policy,内容安全政策），那么eval，new Funtion这些方法都可能无法使用


现在有提案，在语言标准的层面，引入global作为顶层对象，也就是说，在所有环境下，global都是存在的，都可以从它那到顶层对象

垫片库system.global模拟了这个提案，可以在所有环境拿到global

```javascript

//CommonJS的写法
var global = require('system.global')();

//ES6模块的写法
import getGlobal from 'system.global';
const global = getGlobal();

```