-----------------
2015-11-06
-----------------
-----------------
  JS中的闭包
-----------------
闭包的作用个人理解为是突破作用域的限制，从而使局部作用域对其父级作用域是可见的．闭包还有一个有趣的地方是即使在突破作用域限制后，自身仍保持有对其定义环境的上下文的访问权限．

产生闭包的常见方式有通过返回函数，或是返回被赋值为函数的全局变量
## 方式一
<pre>
function fun() {
	var b = "b";
	return function () {
		return b;	
	}
}
var b = fun();
// console.log(b());
b();	// 'b'
</pre>
该段代码通过返回一个匿名函数产生闭包，而在<code>fun()</code>执行环境外，仍然可以访问
变量<code>b</code>

## 方式二
<pre>
var n;
function fun2() {
	var b = 'c';
	n = function () {
		return b;
	}
}
fun2();
n();　// 'c'
</pre>
## 在循环中使用闭包
<pre>
function fun3() {
	var a = [], 
			i;
	for(i = 0; i < 3; i++) {
		a[i] = function () {
			return i;
		};
	}

	return a;
}
var c = fun3();
c[0](); // 3
c[1](); // 3
c[2](); // 3
</pre>
执行的结果似乎有点意外，因为通常估计输出是１，２，３．这里没有按预期输出的原因是，代码中实际是创建了三个闭包，然而它们引用的都是同一个局部变量ｉ．闭包并不会记录它们的值，仅仅是记录它们的执行上下文环境．当循环结束时ｉ的值为３，所以三个函数都指向了这个共同引用．

那个应该如何修改才能根据我们的预期结果输出呢？这里我们需要创建更多的独立作用域，从而使得变量ｉ不是指向同一引用．立即执行函数适合用来创建独立的作用域
<pre>
function fun4() {
	var a = [], 
			i;
	for(i = 0; i < 3; i++) {
		a[i] = (function (x) {
			return function() {
				return x;
			};
		})(i);
	}

	return a;
}
var d = fun4();
d[0](); // 0
d[1](); // 1
d[2](); // 2
</pre>
同样的方式：
<pre>
function fun5 () {
	var a = [], 
			i;
	function makeClosure(x) {
		return function() {
			return x;
		}
	}
	for (i = 0; i < 3; i++) {
		 a[i] = makeClosure(i);
	};
	return a;
}
var d = fun５();
d[0](); // 0
d[1](); // 1
d[2](); // 2
</pre>