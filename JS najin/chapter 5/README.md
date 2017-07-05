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

现在，我们理解了闭包是什么，以及它们时如何工作的，接下来让我们来看看如何在页面中使用闭包

##### 私有变量

闭包的一种常见用法是封装一些信息作为“私有变量” --也就是说，限制这些变量的作用域，在编写面向对象的JavaScript代码时，是无法使用传统的私有变量的：对象的属性对外保持隐藏，但是通过使用闭包，我们可以实现一个可接受的类似的功能，示例代码如下：

```javascrpt

function Ninja(){
	var feints = 0;

	this.getFeints = function(){
		return feints;
	};

	this.feint = fucntion(){
		feints++;
	}
}

var ninja = new Ninja();

ninja.feint();

assert(ninja.getFeints() == 1,'We're able to access the internal feint count.')

assert(ninja.feints === undefined, 'And the private data is in accessible to us'); 

//代码显示，即便我们没有对该变量直接赋值，通过方法我们将该变量增加到1了，我们可以操作feints的值，是因为几遍是该构造器执行完并且已经没有作用域了。fenit变量还是会绑定在 feint()方法声明创建的闭包上，并且可以在feint()方法内进行使用。

```

上述代码允许ninja的状态可以通过一个方法来维护，而无需通过用户方法进行直接访问，因为该变量可以通过闭包存在内部方法，而不是存在于构造器外部的代码。

#### 回调(callback)于计时器(timer)

另外一个使用闭包的最常见情形，是在处理回调或使用计时器的时候，在这两种情况下，函数都是在后期未指定的时间进行一步调用，在这种函数内部，我们经常需要访问外部数据。

闭包可以作为一种访问这些数据的很直观的方式，特别是当我们希望避免创建全局变量来存储这些信息的时候。

`在Ajax请求的callback里使用闭包`

```html

<div id="testSubject"></div>

<button type="button" id="testButton">Go!</button>

<script type="text/javascript">

	jQuery('#testButton').click(function(){
		var elem$ = jQuery('#testSubject');
		elem$.html('Loading...');
		
		jQuery.ajax({
			url: 'test.html',
			success: function(html){
				assert(elem$.'We can see elem$,value the closure for this callback');
				
				elem$.html(html);

			}
		});
	});

</script>
```

`使用$符号作为变量的后缀活前缀，是一个jQuery约定，用于表明该变量引用的是jQuery对象`

在传递给jQuery ajax()方法的参数中，我们定义了一个匿名函数作为响应回调。在这个回调中，我们通过闭包引用了elem$变量，并使用该变量将响应文本填充到<div>元素中。

`在计时器间隔回调(timer interval callback)中使用闭包`

```html

<div id="box">Hello world</div>
<script type="text/javascript">

	function animateIt(elementId){
		var elem = document.getElementById(elementId);
		var tick = 0;
		var timer = setInterval(function(){
			if (tick < 100){
				elem.style.left = elem.style.top = tick + "px";
				tick++;
			}else{
				clearInterval(timer);
				assert(tick == 100,"Tick accessed via a closure.");
				assert(elem,"Element also access via a closure.");
				assert(timer,"Timer reference alse obtained via a closure.");
			}
		},10);
	}

	animateIt('box');

</script>

```

上述代码使用一个独立的匿名函数完成特定元素的动画效果，通过闭包，该函数使用三个变量控制动画过程。

这三个变量(DOM元素的引用，tick计数器，计数器引用)都必须要维持真个动画过程，并且需要能在全部作用域内访问到。

如果我们在全局作用域内保存变量，那么需要为每个动画设置三个变量--否则，用三个变量来跟踪多个动画的状态，那状态就会串了。

没有闭包，同事做多件事情的时候，无论事件处理，还是动画，甚至是Ajax请求，都将是极其困难的。

函数在闭包里面执行的时候，不仅可以在闭包创建的时刻点上看到这些变量的值， 我们还可以对其进行更新，换句话说，闭包不是在创建那一时刻的状态的快照，而是一个真实的状态封装，只要闭包存在，就可以对其进行修改。


### 绑定函数上下文

用call()和apply()方法来操作函数上下文，虽然这种操作也很有用，但对于面向对象代码，它也有潜的危害性。

`给函数绑定一个特定的上下文`

```html

<button id=""test>Click Me!</button>
<script>
	var button = {
		clicked: false;
		click: function(){
			this.clicked = true;
			assert(button.clicked,"The button has been clicked");
		}
	};

	var elem = document.getElementById("test");
	elem.addEventListener("click",button.click,false);
<script>

```

代码测试失败，因为click函数的上下文不是我们所期待的button对象

在本示例中，浏览器的事件处理系统认为函数调用的上下文是事件的目标元素，导致其上下文是<button>元素，而不是button对象。所以我们将click状态设置在错误的对象上了。

将上下文设置为调用事件处理程序时的目标元素，是一个完全合理的默认行为，很多情况下，我们可以对其进行判断，但在本例中，用的就是默认方式，闭包可以帮我们解决这个问题。

通过使用匿名函数，apply()和闭包，我们可以强制让特定的函数在调用时都使用特定所需的上下文。

`给事件处理程序绑定特定的上下文`

```javascript

function bind(context,name){
	return function(){
		return context[name].apply(context,arguments);
	}
}

var button = {
	clicked: false,
	click: function(){
		this.clicked = true;
		assert(button.clicked,"The button has been cllicked");
		console.log(this);
	}
};


var elem = document.getElementById("test");
elem.assEventListener("click",bind(button,"click"),false);

```

bind方法，该方法用于创建并返回一个匿名函数，该匿名函数使用apply()调用了原始函数，以便我们可以强制将上下文设置成我们想要的任何对象。本例中，传递给bind()的第一个参数就是要设置的上下文对象。上下文(context)和方法名称(name),通过匿名函数的闭包进行传入，在函数结束时进行调用，而匿名函数闭包则包含了传递给bind()的参数。

接下来，在建立时间处理程序时，我们使用了bind()方法来指定事件处理程序，而不是直接使用button.click。这会让包装的匿名函数成为事件处理程序。当单击按钮时，将调用匿名函数，然后反过来再调用click方法，同时将上下文强制设置成button对象。

该bind()函数是Prototype流行库中其中的一个函数的简化版本，该库可以使得代码变得整洁并使用经典的面向对象方式进行编程。

`在Prototype库中，函数bind代码的示例`

```javascript

Function.prototype.bind = function(){
	var fn = this, args = Array.prototype.slice.call(arguments), object = arg.shift();

	return function(){
		return fn.apply(object,a);
	};
};

var myObject = {};
function myFunction(){
	return this == myObject;
}

assert( !myFunction(), "Context is not set yet");

var aFunction = myFunction.bind(myObject)

assert( aFunction(), "Context is set properly");


```

将自身方法作为Function的prototype属性的属性，以便将该方法附加到所有的函数上，而不是声明一个全局作用域的方法。

重要的是要意识到，Prototype的bind()(或是我们自己实现的)，并不意味着它是apply()或call()的一个替代方法，记住，该方法的潜在目的是通过匿名函数和闭包控制后续执行的上下文，这个重要的区别使apply()和call()对事件处理程序和定时器的回调进行延迟执行特别有帮助。