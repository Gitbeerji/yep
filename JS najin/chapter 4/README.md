### 匿名函数

通常匿名函数的使用情况是，创建一个供以后使用的函数。例如，将匿名函数保存在一个变量里，将其作为一个对象的方法，或者是将匿名函数作为一个回调(例如，timeout或事件处理程序)。在这些情况下，该函数并不需要名称进行引用。

JavaScript的强大威力依赖于是否将其作为函数式语言进行使用，函数式编程专注于：少、通常无副作用、将函数作为程序代码的基础构建块

为了不让不必要的函数名称污染全局命名空间，我们将创建大量的小型函数进行传递，而不是创建包含大量命令语句的大型函数。


### 递归(recursion)

当函数调用自身，或者调用另外一个函数，但这个函数的调用树中又调用了自己式，递归就发生了。

对任何类型的程序来说，递归都是一个非常有用的技术，递归在有大量数学计算的程序里是最有用的，很多数学公式在本质上都是递归，而且，对树进行遍历的时候，递归也是非常有用的，这是一个可能在Web程序中出现的构造。

#### 普通命名函数中的递归

有很多常见的递归函数示例，其中一个是用于检测回文的--相当于递归技术的"Hello world！"。

回文的非递归定义是"一个短语，不管从哪个方向读都是相同的"，可以用他来实现一个函数，用于创建反向字符串并和字符串本身进行比较。但是复制字符串从多方面来说不是简洁的解决方案，其中一个原因就是需要分配并创建新的字符串。

通过回文的更多数学定义，我们可以想出一个更简洁的解决方案。这些定义 如下所示。
1. 单个和零个字符都是一个回文
2. 如果字符串的第一个字符和最后一个字符相同，并且除了两个字符以外剩余的其他字符也是一个回文的话，我们称原字符是一个回文

基于该定义的代码实现如下： 

```javascript

function isPalindrome(text) {
	if(text.length <= 1 ) return true;
	if(text.charAt(0) != text.charAt(text.length -1)) return false;
	return isPalindrome(text.substr(1,text.length - 2));
}


```

#### 关于递归

函数的递归有两个条件： 引用自身，并且有终止条件。

很显然函数调用了自身，所有满足第一个条件，由于每次迭代参数n的值都会减少，它迟早会达到递归终止的条件，所以也满足了第二个条件。

“递归”函数不能终止于无限循环！

### 方法中的递归

```javascript

var ninja = {
	chirp: fucntion(n) {
		return n > 1 ? ninja.chirp(n - 1) + "-chirp" : "chirp";
	}
}

```

上述代码中，我们将递归函数定义成一个匿名函数，并将其引用到您家对象的chirp属性，在该属性内，我们通过对象的ninja.chirp()属相递归调用了函数自身。

#### 引用丢失的问题

一个进行递归调用的对象属性引用，与函数的实际名称不同，这种引用可能是暂时的，这种依赖方式会导致我们很混乱

```javascript

var ninja = {
	chirp: function(n){
		return n > 1 ? ninja.chirp(n - 1) + "-chirp" : "chirp";
	}
}


var samurai = { chirp: ninja.chirp };

ninji = {};


```

![引用丢失](https://ooo.0o0.ooo/2017/06/22/594b7f2b33bb5.jpg)


通过完善原本对递归函数的粗略定义，我们可以修复解决这个问题。在匿名函数中不再使用显示的ninja引用，而是使用函数上下文(this)进行引用，示例如下：

```javascript

var ninja = {
	chirp: function(n){
		return n > 1 ? this.chirp(n - 1) + "-chirp" : "chirp";
	}
}

```

### 内联命名函数

现在又有了另外一个问题，解决方案取决于： 给对象定义方法时，该方法名称必须是chirp()。如果属性的名字不一样会怎么样呢？ 或者如果该函数的另一个引用不是对象属性又如何？我们的解决方案只能在特定的情况下才能使用，也就是将函数作为方法并且所有方法的属性名都一样。我们可以开发出更加通用的匿名函数吗？

考虑给匿名函数取个名字，这些方法不再是匿名函数(anonymous),最好叫它们内联函数(inline function),从而避免使用匿名命名函数(anonymous named function)而产生矛盾.

```javascript

var ninja = {
	chirp: function signal(n) {
		return n > 1 ? signal(n - 1) + "-chirp" : "chirp";	
	}
};

var samurai = {chirp: ninja.chirp};

ninja = {};

```

给内联函数取名带来的威力可以进一步扩大，它甚至可以用于普通的变量赋值，如下所示，可以看到一些奇怪的结果

```javascript

var ninja = function myNinja(){
//声明一个命名内联函数，并将其赋值给一个变量
	assert(ninja == myNinja,
		"This function is named tow things at once!");
	//在内联函数中，验证两个名字是等价的。
};

ninja();
//调用函数执行内部验证

assert(typeof myNinja == "undefined",
	"But myNinja isn't defined outside of the function.");
//验证内联函数的名称在内联函数外部是不可用的

```

`注意：这就是为什么要讲全局函数作为window的方法进行创建的原因，不适用window的属性，我们没有办法引用这些函数`

虽然给内联函数取名，可以解决这些函数在递归引用方面的问题(可以说这种方式比this更清晰)，但它的作用是有限的。

### callee属性

`警告: callee属性在即将到来的新版JavaScript上会被去除，而且ECMAScript 5 标准在strict模式下也禁止使用该属性了，不过，在当前浏览器中还是可以使用该属性的，但它的使用不具前瞻性，我们可能不希望在新代码中使用callee，不过可能在现有的代码中遇到`

```javascript 

var ninja = {
	chirp: function(n){
		return n > 1 ? arguments.callee(n - 1) + "-chirp" : "chirp";
	}
}

assert(ninja.chirp(3) == "chirp-chirp-chirp",
	"arguments.callee is the function itself.");

```


### 将函数视为对象

JavaScript中的函数和其他语言中的函数不同，JavaScript赋予了函数很多特性，其中最重要的特性之一是将函数作为第一性对象。

先从将函数赋值给变量开始

```javascript

var obj = {};        
var fn = function(){};       
assert(obj && fn; "Both the object and function exist");

```

注意： 要记住一件重要的事情： function(){}定义后面的分号，在所有语句结束时，使用分号是一个很好的做法，尤其是在变量赋值之后，匿名函数也不例外，压缩代码时，分号的妥善安置会为压缩技术提供很大的灵活性。


函数和其他对象一样，我们可以给函数添加属性。

### 函数存储

有时候，我们可能需要存储一组相关但又独立的函数，事件回调管理是最明显的例子，先这个集合添加函数时，我们面临的挑战是要确定哪些函数在集合中不存在，而应该添加，以及哪些函数已经存在并且不需要再添加了。

显而易见但天真的做法是，将所有的函数保存在一个数组里，然后遍历数组检查重复的函数，只不过这种方式很一般。

我们可以利用函数的属性，给函数添加一个附加属性从而实现上述目的。

```javascript 

var store = {
	nextId: 1,
	cache: {},
	add: function(fn){
		if(!fn.id){
			fn.id = store.nextId++;
			return !!(store.cache[fn.id] = fn);
		}
	}
};

function ninja(){}

assert(store.add(ninja),"Function was safe added");

assert(!store.add(ninja),"But it was only added once");

```

通过追踪函数的一个属性，可以达到跟踪函数的目的

另一个有用的技巧是，通过暴露函数属性，我们可以对函数自身进行修改，该技巧可以用于记住以前计算的值，以便在未来计算时节约时间。


#### 自记忆函数

缓存记忆(memoization)是构建函数的过程，这种函数能够记住先前计算的结果。通过避免已经执行过的不必要复杂计算，这种方式可以显著提高性能。

#### 缓存记忆昂贵的计算结果

```javascript

function isPrime(value) {    
   if (!isPrime.anwers) isPrime.answer = {};    
   if (isPrime.answer[value] != null) {   
      return isPrime.answers[value];   
   }   
   var prime = value != 1; // 1 can never be prime;   
   //判断value是否为1，不为1 prime = true 为1 prime = false；   
   for (var i = 2; i < value; i++) {   
      if (value % i == 0) {   
         prime = false;   
         break;   
      }   
   }   
   return iPrime.answer[value] = prime;   
}

```

缓存记忆有两个主要优点   

- 在函数抵用获取之前计算结果的时候，最终用户享有性能优势   
- 发生在幕后，完全无缝，最终用户和页面开发人员都无需任何特殊操作或为此做出任何额外的初始化工作    

缺点

- 为了提高性能，任何类型的缓存肯定会牺牲内存
- 纯粹主义者可能认为缓存这个问题不应该与业务逻辑放在一起，一个函数或方法应该只做一件事，并把它做好
- 汉南测试或测量一个算法的性能

#### 缓存记忆 DOM元素

通过元素的标签查询DOM元素集是相当常见的操作，但是性能可能不是特别好。我们可以利用新发现的缓存记忆特性创建一个缓存，用于保存已经匹配到的元素集。

```javascript

function getElements(name) {
   if (!getElement.cache) getElements.cache = {};
   return getElements.cache[name] = 
      getElememnts.cache[name] ||
      document.getElementsByTagName(name);
}


```
记忆(缓存)代码非常简单，不会对整个查询过程增加太多的复杂性。   

我们可以将状态和缓存信息存储在一个封装的独立位置上，不仅在代码组织上有好处，而且外部存储或缓存对象无需污染作用域，就可以获取性能提升。


#### 伪造数组方法

有时我们可能想创建一个包含一组数据的对象，如果只是集合，只需要创建一个数组即可，但是某些情况下，除了集合本身，可能会有更多的状态需要保存--比如与集合项有关的一些元数据。

一种选择可能是，每次创建对象新版本的时候都创建一个新数组，然后将元数据作为属性或方法添加到这个新数组上，不过这个方式太慢了，更不说乏味了。


```javascript

var elems = {
	length: 0,
	add: function(elem) {
		Array.prorotype.push.call(this,elem);
		//push会增加length属性的值(会认为他是数组length属性)
	},
	gather: function(id) {
		this.add(document.getElementById(id));
	}
};

```

这一节演示的边缘型“不法”行为，不仅揭示了上下文的超强特性，在函数参数处理的复杂性方面，给我们上了一堂很好的深入探讨课。

### 可变长度的参数列表

JavaScript的使用非常灵活，JavaScript灵活且强大的特性之一是函数可以接受任意数量的参数，这种灵活性可以让开发人员很好地进行函数控制，也因此才能编写出他们的应用程序

利用灵活的参数列表

- 如何给能接受任意参数的函数提供多个参数
- 如何使用变长的参数列表实现函数重载
- 如何理解并使用参数列表的length属性

由于JavaScript没有函数重载(一种我们可能已经习惯的面向对象语言的功能)，参数列表的灵活性是获得其他语言类似重载功能的关键所在。

#### 使用apply()支持可变参数

所有的语言，有一些我们经常要做的事情，似乎都被语言的开发人员莫名其妙的忽略了，JavaScript也不例外。

其中有一件奇怪的事情，就是查找数组中的最小值或最大值。在JavaScript中似乎没哟这两种功能，但如果随意探索的话，可能会发现Math对象有两个名为min() max()的方法。

测试后发现每个方法都需要可变长度的参数列表，而不是数组，不一起提供是多么愚蠢。

也就是说，Math.max()的调用，需要向如下这样：

var biggest = Math.max(1,2);    
var biggest = Math.max(1,2,3);
var biggest = Math.max(1,2,3,4,5,6,7,8,9,10);

思考一种简单的方式，支持将数组作为一个可变长度的参数列表。

apply()方法！！！！

```javascript 

function smallest(array){
	return Math.min.apply(Math,array);
}

function largest(array){
	return Math.max.apply(Math,array);
}

```

将Math对象指定为上下文，这是没有必要的(不管是什么上下文，min()和max()方法都能正常使用)，但在这种情况下，没有理由不让代码保持整洁。

至此，我们知道了在调用函数时如何使用可变长度的参数列表，那就让我们看看如何声明自己的函数来接受这种参数吧。

#### 函数重载

##### 检测并遍历参数

在其他更纯的面向对象语言里，方法重载通常是通过在同名方法(但不同参数)里声明不同的实现来达到目的的，但在JavaScript中并非如此，在JavaScript中，我们重载函数的时候只用一个实现，只不过这个实现内部是通过传入参数的特性和个数进行相应修改来达到目的的。

如下代码中，我们要把多个对象的属性合并到一个跟对象上，这可能是影响继承的一个重要工具。

```javascript
	
	function merge(root){
		for (var i = 1; i < arguments.length; i++) {
			for (var key in arguments[i]) {
				root[key] = arguments[i][key];
			}
		}
		return root;
	}

	var merged = merge(
		{name: "Batou"},
		{city: "Niihama"});

```
在JavaScript中，没有强制函数声明多少个参数就得传入多少参数，函数是否可以成功处理这个参数(或缺少的参数)完全取决于函数本身的定义，不过在这方面JavaScript没有强加规则。


访问和遍历arguments集合特性是一种创建复杂且智能方法的强大机制，我们可以用它来检查任意函数的传入参数，以便让函数更灵活地操作这个参数，即使我们事先不知道要传入的参数到底是什么。

##### 对arguments列表进行切片(slice)和取舍(dice)

下一个示例中，我们构建一个函数，将第一个参数与剩余参数的最大值进行相乘，这可能不是特别适用于应用程序，但它是展示更多处理argumens参数技术的例子

```javascript

function multiMax(multi){
	return multi * Math.max.apply(Math,
		Array.prototype.slice.call(arguments,1));
		//强制Array的slice()方法将arguments参数视为一个真正的数组，即使它不是。
}


```

##### 函数重载方式

定义一个函数重载时-- 基于传递的参数定义一个有很多不同功能的函数--很容易想像，根据目前我们所学的这种机制，检查参数列表并使用if-then后else-if子语句执行不同的行为，我们可以很容易实现一个函数，通常这种方法很好用，尤其在代码处理很简单的情况下。

但是一旦事情开始变得开始有点复杂的话，大量的函数使用很快回导致代码笨拙。在本章节的剩余部分，我们将探讨一种可以创建多个相同名称函数的技术，但是根据期望参数的不同，每个函数也不同--可以写成独立且独特的匿名函数，而不是作为一个整体的if-the-else-if块。

##### 函数的length属性

length属性等于该函数声明时需要传入的形参数量

因此对于一个函数，在参数方面我们可以确定两件事     
- 通过其length属性，可以知道声明了多少命名参数   
- 通过arguments.length，可以知道在调用时传入了多少参数

让我们看看如何利用这个属性构建一个函数，利用参数个数的差异创建重载函数。

##### 利用参数个数进行函数重载

基于传入得参数，有很多种方法可以判断并进行函数重载，一种通过的方法是，根据传入的参数的类型执行不同烦人操作，另一种方法是，可以通过某种特定的参数是否存在来进行判断。还有一种方法是通过传入参数的个数进行判断。

假设在对象上有一个方法，根据传入参数的个数来执行不同的操作。如果想要冗长且完整的函数，则会像如下这样：

```javascript

var ninja = {

	whatever: function() {
		swith (arguments.length) {
			case 0: 
				break;
			case 1:
				break;
			case 2:
				break;
		}
	}

}

```

在这种方式中，通过arguments参数获取实际传入的参数个数进行判断，每一种情况都会执行不同的操作。但这种方式不是很整洁

让我们假设另一种方法。 如果按照如下思路， 添加想要的重载方式怎么样呢：

```javascript 

var ninja = {};    
addMethod(ninja, 'whatever', function(){})；    
addMethod(ninja, 'whatever', function(a){});   
addMethod(ninja, 'whatever', function(a,b){});

```
在这里，先创建一个对象，然后使用同样的名称(whatever)将方法添加到该对象上，只不过每个重载的函数都是单独的，注意每个重载函数的参数个数都不相同，通过这种方式，我们真正为每个重载都创建了一个独立的匿名函数。

```javascript

function addMethod(object, name, fn) {
	var old = object[name];
	//保存原有的函数，因为调用的时候可能不匹配传入的参数
	object[name] = function(){
		//console.log('xx');
		//通过了函数的嵌套，如果满足第一层条件就返回那个函数，否则就慢慢向下嵌套循环，知道符合条件，返回那个函数。
		if(fn.length == arguments.length)
			return fn.apply(this,arguments);
			//如果该匿名函数的参数个数和实数个数匹配，就调用该函数
		else if (typeof old == 'function')
			return old.applay(this.arguments);
			//如果传入的参数不匹配，则调用原有的参数 
	}

```

addMethod()的第一次调用将创建一个新匿名函数，传入零个参数进行调用的时候将会调用该fn函数，由于此时ninja是一个新对象，所有这时候不用担心之前创建的方法。

第二次调用addMethod()的时候，首先将之前的同名函数保存到一个变量old中，然后将新创建的匿名函数作为方法。新方法首先检查传入的参数个数是否为1，如果是，就调用刚才传入的fn函数；如果不是，则重新调用存储在old上的函数，重新调用该函数时，将会再次检查参数个数是否为零，继而调用参数个数为零的fn版本的函数。

第三次调用addMethod()的时候，传入了一个接受两个参数的fn函数，然后判断逻辑相同：创建一个尼米你个函数做方法，判断如果传入参数的个数为2个，则调用2个参数的fn函数，并推迟之前创建1个参数的函数。

这种方式就像剥洋葱皮一样，每一层都检查参数个数是否匹配，如果不匹配的话，就推迟上一层创建的函数。

这里有一些技巧是关于内部匿名函数是如何访问到old和fn的，它涉及到一个叫闭包的概念。

这是个绝佳的技巧，因为这些绑定函数实际上并没有存储于任何典型的数据结构中，而是在闭包里进行存储。

应该注意的是，在使用这个特定的技巧时，需要注意一下几点。

- 重载只适用于不同数量的参数，但并不区分类型、参数名称或其他东西，这些才是我们经常想做的事情。
- 这样的重载方法会有一些函数调用的开销，我们要考虑在高性能的请况。

#### 函数判断

如何判断一个给定对象是一个函数的示例，并且是可以调用的。这看似是一个简单的任务，但确实有跨浏览器的问题。

通常 typeof语句就可以完全满足这一要求，

```javascript

function ninja(){}

assert(typeof ninja == 'function',
	'Functions have a type of function');

```

这个应该是判断给定对象是否是函数的一个典型方式，如果我们测试的确实是一个函数，这种方式一直都是有效的的。但也有一些情况，这个测试可能会返回错误的结果，需要注意如下事项.

- Firefox --在HTML的<object>元素上使用typeof的话，会返回function，而不是我们期望的object。
- Internet Explorer -- 如果对其他窗口(比如iframe)的不存在对象进行类型判断，该类型会返回unknown
- Safari -- Safari认为 DOM 的NodeList是一个function。 所以 typeof document.body.childNodes == 'function'

这些特定的情况将会导致我们的代码出现问题，我们需要一个在所有目标浏览器下都能工作的解决方案，以便让我们判断这些特殊函数(或非函数)是否能够正确报告自身。

这里有很多可能的探索途径，可惜的是几乎所有的方式都不能完美解决这个问题。例如，我们知道，函数有applay()和call()方法但是 Internet Explorer 的这些问题函数并没有这些方法。 有一个相当不错的方式是，将函数转换为一个字符串，根据其序列化值判断其类型，代码入下

```javascript 

function isFunction(fn){
	return Object.prototype.toString.call(fn) == "[object Function]";
}

```

