### 闭包

传统上来说，闭包是纯函数式编程语言的一个特性，让闭包跨越到主流语言的开发上尤其让人鼓舞，因为他们能够大大简化复杂的操作，很容易在一些JavaScript库以及其他高级代码库中找到闭包的使用。

#### 闭包是如何工作的

简单地说，闭包(closure)是一个函数在创建时允许该自身函数访问并操作该自身函数之外的变量时所创建的作用域，换句话说，闭包可以让函数访问所有的变量和函数，只要这些变量和函数存在于该函数声明时的作用域内就行。

```javascript 

var outerValue = 'ninja';

var later;

function outerFunction(){
	var innerValue = 'samurai';

	function innerFunction(){
		assert(outerValue,'I can see the ninja.');
		assert(innerValue,'I can see the samurai.');
	}

	later = innerFunction;
}

outFunction();

later();


```

在外部函数中声明innerFunction()的时候，不仅是声明了函数，还创建了一个闭包，该闭包不仅包含函数声明，还包含了函数声明的那一时刻点上该作用域中的所有变量。   

最终当 innerFunction()执行的时候，当时声明的作用域已经消失了，通过闭包，该函数还是能够访问到原始作用域的。

这就是我们所说的闭包，如果我们愿意，针对在函数声名那一时刻点的作用域内的所有函数和变量，闭包创建了一个‘安全气泡’，因此函数获得了执行操作所需要的所有东西。


```javascript

var outerValue = 'ninja';
var later;

function outerFunction() {
	var innerValue = 'samurai';

	function innerFunction(paramValue) {
		assert(outerValue,'Inner can see the ninja.');
		assert(innerValue.'Inner can see the samurai');
		assert(paramValue,'Inner can see the wakizashi');
		assert(tooLate,'Inner can see the ronin');
	}

	later = innerFunction;
}

assert(!tooLate,'Outer can't see the ronin.');

var tooLate = 'ronin';

outerFunction();

later('wakizashi');

```

概念  

- 内部函数的参数是包含在闭包中的   
- 作用域之外的所有变量，即使是函数声明之后那些声明，也都包含在闭包中
- 相同的作用域内，尚未声明的变量不能提前引用。

这种方式在存储和引用信息方面会直接影响性能。重要的是要记住，每个通过闭包进行信息访问的函数都有一个'锁链'，如果我们愿意， 可以在上面附加任何信息，尽管闭包非常有用，但是他们的使用并非完全没有开销，使用闭包时，闭包里的信息会一直保存在内存里，知道这些信息确保不再被使用(可以安全进行垃圾回收)，或页面卸载时，JavaScript引擎才能清理这些信息。

#### 使用闭包

