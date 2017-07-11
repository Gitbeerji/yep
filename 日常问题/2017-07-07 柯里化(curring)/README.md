### 柯里化(curring)

由JavaScript忍者秘籍第五章，第四节， 5.4 偏应用函数而引出的概念。

书中的陈述：    

“分布应用”一个函数是一项特别有趣的技术，在函数调用之前，我们可以预先传入一些函数。实际上，偏应用函数返回一个含有预处理参数的新函数，以便后期可以挑用。

这类代理函数--代理的是另一个函数，并且在执行的时候回调用所代理的函数--试试我们上街所使用的的在函数调用时"绑定"特定上下文的技术。这里只是把相同的技术用在不同的地方而已。

这种在一个函数中首先填充几个参数(然后再返回一个新的函数)的技术成为柯里化(curring).

Prototype库中bind()方法的实现：   
```javascript

Function.prototype.bind = function(){
	var fn = this, args = Array.prototype.slice.call(argumens), object = args.shift();

	return function(){
		return fn.apply(object,args.concat(Array.prototype.slice.call(arguments)));
	};
};

```

柯里化函数示例

```javascript

Function.prototype.curry = function(){
	var fn = this, args = Array.prototype.slice.call(arguments);

	return function(){
		return fn.apply(this, args.concat(Array.prototype.slice.call(arguments)));
	};
};

```

更复杂的“分部”函数

```javascript

Function.prototype.paritial = function(){
	var fn = this,args = Array.prototype.slice.call(arguments);

	return function(){
		var arg = 0;
		for (var i = 0; i < args.length && arg < arguments.length;i++) {
			if(args[i] === undefined) {
				args[i] = arguments[arg++];
			}
		}

		return fn.apply(this,args);
	};
}


String.prototype.csv = String.prototype.split.partial(/,\s*/);

```

该实现的本质类似于Prototype的curry()方法，但它有几个重要的差异。值得注意的是，用户可以在参数列表的任意位置指定参数，然后在后续的调用中，根据遗漏的参数值是否等于undefined来判断参数的遗漏。要实现这种功能，我们添加了参数合并功能。很有效果，遍历传入的所有参数，判断相应的参数是否遗漏了(是否undefined)，然后沿着顺序填充遗漏的参数。
