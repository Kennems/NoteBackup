# LeetCode刷题知识点

## JavaScript闭包

在JavaScript中，**闭包（closure）**是指**函数**与其**词法环境**的组合。它允许函数在其**定义的词法作用域**之外**访问变量**，并保持对这些**变量的引用**，即使在函数外部被调用时仍然有效。

闭包由两个主要部分组成：

- 函数：闭包是一个函数，它定义了一些变量和逻辑。
- 词法环境：词法环境是在函数定义时创建的作用域，它包含了函数中定义的变量和它们的值。

从**广义**上来说，任何JavaScript函数都是闭包，比如在浏览器中，任何一个函数都有一个global对象，那就是window，而在node环境中，每个函数也有一个global对象。

从**狭义**上来说，JavaScript中一个函数，如果访问了外层作用域的变量，那么它是一个闭包。

通俗易懂地讲，一个普通的函数function，如果它可以访问外层作用于的自由变量，那么这个函数就是一个闭包。

> **Activation Object**是什么？ 当一个函数执行的时候，会创建一个Activation Object。该对象会存储该函数体内的变量以及arguments等信息。

```js
function init() {
	var name = "Name";
	function displayName() {
		// displayName() 是内部函数， 一个闭包
		alert(name);
	}
	displayName();//没有自己的局部变量，但是可以访问到外部函数的变量， 所以displayName()可以使用夫函数init()中声明的变量name
}
init();
```

