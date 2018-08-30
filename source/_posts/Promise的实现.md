---
title: Promise的实现
date: 2018-08-30 15:31:55
tags:
- Promise
- ES6
- 异步
categories: javascript
copyright: true
top:
---

>如何实现Promise

<!--more-->

## 什么是Promise? ##

>primise是异步编程的一种解决方法，可以避免回调地狱的问题，最早由社区提出和实现，现为ES6语言标准。所谓promise，字面意思是承诺，简单说是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果；从语法上说，promise是一个对象，从它可以获取异步操作的消息，promise提供了统一的API，各种异步操作都可以用同样的方法进行处理。

关于promise的基础知识，网上很多博客都有讲解，也可以参考参考阮一峰老师的[ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/promise)。

---

## Promise的实现 ##

### 基础实现 ###

```
    function Promise(fn){
        //deferreds用来存储回调函数
		var value = null, deferreds = [];
        //then相当于订阅过程
		this.then =function(onFulfilled){
			deferreds.push(onFulfilled);
		};
        //resolve相当于发布过程
		function resolve(value){
			deferreds.forEach(function(deferred){
				deferred(value);
			});
		}
		fn(resolve);
	}
```

### 避免resolve执行时deferreds为空 ###

避免传入的不是异步函数，resolve方法会先执行，此时没有调用then，使得defferreds队列还是空的，不合预期。

```
    function Promise(fn) {
		var value = null,
			deferreds = [];
		this.then = function(onFulfilled) {
			deferreds.push(onFulfilled);
		};
        //这里将resolve放到了栈底，所以then会先执行（如果有then的话）
		function resolve(value) {
			setTimeout(function() {
				deferreds.forEach(function(deferred) {
					deferred(value);
				});
			}, 0);
		}
		fn(resolve);
	}
```

### 状态引入 ###

如果当异步操作执行成功之后，内部resolve已经执行完毕，再调用then方法注册回调函数就不会再执行了。
要解决这个问题，需要引入规范中的三个状态：pending、fulfilled、rejected。如果你了解过promise的基础知识，这三个状态肯定不会陌生了。

```
    function Promise(fn) {
		var value = null,
		    state = null,
			deferreds = [];
		this.then = function(onFulfilled) {
			if(state==='pending'){
				deferreds.push(onFulfilled);
				return this;
			}
			onFulfilled(vaule);
			return this;
		};

		function resolve(newValue) {
           //当resolve时改变状态为fulfilled
			value = newValue;
			state = 'fulfilled';
			setTimeout(function() {
				deferreds.forEach(function(deferred) {
					deferred(value);
				});
			}, 0);
		}
		fn(resolve);
	}
```

调then的时候做个判断，如果是pending说明resolve方法还没执行，那么我们将回调函数加到队列等待resolve即可，如果是fulfilled，说明resolve已经执行，那么我们直接执行新加入的回调函数。

### 串型promise,实现链式调用 ###

```
    function Promise(fn) {
		var value = null,
			state = null,
		deferreds = [];
		this.then = function(onFulfilled) {
			return new Promise(function(resolve) {
				handle({
					onFulfilled: onFulfilled || null,
					resolve: resolve
				});
			});
		};

		function handle(deferred) {
			if (state === 'pending') {
				deferreds.push(onFulfilled);
				return;
			}
			var ret = deferred.onFulfilled(vaule);
			deferred.resolve(ret);
		}


		function resolve(newValue) {
			if(newValue && (typeof newValue === 'Object' || typeof newValue === 'function')){
				var then=newValue.then;
				then.call(newValue,resolve);
				return;
			}
			value = newValue;
			state = 'fulfilled';
			setTimeout(function() {
				deferreds.forEach(function(deferred) {
					handle(deferred);
				});
			}, 0);
		}
	}
```

这部分理解起来会比较困难，then中返回一个新的promise实例，在新实例中调用内部方法handle，handle传入一个回调函数和一个当前新promise的resolve方法构成的对象。内部方法判断状态，如果为pending状态，此时resolve还未执行，则将回调函数入队列，否则直接调用传入的回调函数，并将回调的返回值传入resolve方法。针对返回值的不同，可能是一个新的promise，所以在resolve方法中做一个判断，如果是对象或是函数则取出新promise的then，并调用这个then；如果不是对象或是函数，则直接resolve。

### 增加rejected状态 ###

```
    function Promise(fn) {
		var value = null,
			state = null,
		deferreds = [];
		this.then = function(onFulfilled) {
			return new Promise(function(resolve) {
				handle({
					onFulfilled: onFulfilled || null,
					onRejected: onRejected || null,
					resolve: resolve,
					reject: reject
				});
			});
		};

		function handle(deferred) {
			if (state === 'pending') {
				deferreds.push(onFulfilled);
				return;
			}
			var cb=state==='fulfilled'?deferred.onFulfilled : deferred.onRejected;
			if(cb===null){
				cb=state==='fulfilled'?deferred.resolve : deferred.reject;
				cb(value);
				return;
			}
			var ret = cb(value);
			deferred.resolve(ret);
		}


		function resolve(newValue) {
			if(newValue && (typeof newValue === 'Object' || typeof newValue === 'function')){
				var then=newValue.then;
				then.call(newValue,resolve);
				return;
			}
			value = newValue;
			state = 'fulfilled';
			finale();
		}

		function reject(reason){
			state = 'rejected';
			value = reason;
			finale();
		}

		function finale(){
			setTimeout(function() {
				deferreds.forEach(function(deferred) {
					handle(deferred);
				});
			}, 0);
		}

		fn(resolve,reject);
	}
```

### 处理resolve和reject的回调函数异常 ###

```
     //修改handle函数
    function handle(deferred) {
			if (state === 'pending') {
				deferreds.push(onFulfilled);
				return;
			}
			var cb=state==='fulfilled'?deferred.onFulfilled : deferred.onRejected,ret;
			if(cb===null){
				cb=state==='fulfilled'?deferred.resolve : deferred.reject;
				cb(value);
				return;
			}
			try{
				ret=cb(value);
				deferred.resolve(ret);
			}catch(e){
				deferred.reject(e);
			}
		}
```

到这里，promise的实现基本完成了，其实刚开始理解起来并不容易，陆陆续续看过几篇博主的实现过程，多多少少都有些不同，但整体思路是一致的，如果不能很好理解，就先记录下来，自己跟着实现一遍，再慢慢去理解会好很多。文章的最后附上几个我认为写的不错的、有关promise实现的博客链接，希望对你有帮助。

参考链接：[实现一个自己的promise](https://blog.csdn.net/yibingxiong1/article/details/68075416)、[手把手教你实现Promise](https://www.jianshu.com/p/a89a8e5d6636)、[实现一个完美符合Promise/A+规范的Promise](https://github.com/forthealllight/blog/issues/4)、[ES6实现完整Promise](https://segmentfault.com/a/1190000012664201).



