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