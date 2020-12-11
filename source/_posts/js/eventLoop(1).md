---
title: 从做题弄懂eventLoop(一)
date: 2020-11-22 16:00
categories: javascript
type: 'tags'
tags: ['javascript', 'event loop']
---
## 先来一道面试题
### 第一题(美团面试原题)

```js
console.log(1);

setTimeout(() => {
  console.log(2);
  Promise.resolve().then(() => {
    console.log(3)
  });
});

new Promise((resolve, reject) => {
  console.log(4)
  resolve(5)
}).then((data) => {
    console.log(data);
    Promise.resolve().then(() => {
        console.log(8)
    });
})

setTimeout(() => {
  console.log(6);
})

console.log(7);
```
请问最后的输出结果是什么?


## eventloop解析
### 1.关于javascript

javascript是一门**单线程**语言，在最新的HTML5中提出了Web-Worker，但javascript是单线程这一核心仍未改变。所以一切javascript版的"多线程"都是用单线程模拟出来的，一切javascript多线程都是纸老虎！
### 2.javascript事件循环

既然js是单线程，那就像只有一个窗口的银行，客户需要排队一个一个办理业务，同理js任务也要一个一个顺序执行。如果一个任务耗时过长，那么后一个任务也必须等着。那么问题来了，假如我们想浏览新闻，但是新闻包含的超清图片加载很慢，难道我们的网页要一直卡着直到图片完全显示出来？因此聪明的程序员将任务分为两类：

- 同步任务
- 异步任务

当我们打开网站时，网页的渲染过程就是一大堆同步任务，比如页面骨架和页面元素的渲染。而像加载图片音乐之类占用资源大耗时久的任务，就是异步任务。关于这部分有严格的文字定义，但本文的目的是用最小的学习成本彻底弄懂执行机制，所以我们用导图来说明：
![异步任务执行](https://user-gold-cdn.xitu.io/2017/11/21/15fdd88994142347?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
导图要表达的内容用文字来表述的话：

- 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
- 当指定的事情完成时，`Event Table`会将这个函数移入`Event Queue`。
主线程内的任务执行完毕为空，会去`Event Queue`读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的`Event Loop(事件循环)`。
#### 第二题
```js
console.log(1)
setTimeout(() => {
    console.log(2)
})
let intervalTimer = setInterval(() => {
    console.log(3)
})
setTimeout(() => {
    console.log(4);
    clearInterval(intervalTimer)
})
console.log(5)
```

### 异步任务
除了广义的同步任务和异步任务，我们对任务有更精细的定义：

- `macro-task`(宏任务)：包括整体代码`script`，`setTimeout`，`setInterval`, `setImmediate`,`I/O`、`UI交互事件`
- `micro-task`(微任务)：`Promise`，`process.nextTick`,`MutaionObserver`

所以整个最基本的**Event Loop**如图所示：
![event loop](https://pic3.zhimg.com/80/v2-fdd9322a0cabafa7d3461e5d25718586_1440w.jpg)
- queue可以看做一种数据结构，用以存储需要执行的函数
- timer类型的API（`setTimeout`/`setInterval`）注册的函数，等到期后进入task队列（这里不详细展开timer的运行机制）
- 其余API注册函数直接进入自身对应的`task/microtask`队列
- Event Loop执行一次，从task队列中拉出一个task执行
- Event Loop继续检查`microtask`队列是否为空，依次执行直至清空队列

#### 第三题
```js
setTimeout(function(){
	setTimeout(function(){
		console.log(1);
	}, 100);
	new Promise(resolve => {
		console.log(2);
		resolve(3);
		setTimeout(function(){
			console.log(4);
		}, 1);
	}).then((number) => {
		setTimeout(function(){
			console.log(5);
		}, 0);
		console.log(number)
	})
	console.log(6);
}, 0);

setTimeout(function(){
	console.log(7);
}, 100);

console.log(8);
```
### 第四题
```js
console.log(1)

setTimeout(() => {
    console.log(2)
    new Promise(resolve => {
        console.log(4)
        resolve()
    }).then(() => {
        console.log(5)
    })
})

new Promise(resolve => {
    console.log(7)
    resolve()
}).then(() => {
    console.log(8)
})

setTimeout(() => {
    console.log(9)
    new Promise(resolve => {
        console.log(11)
        resolve()
    }).then(() => {
        console.log(12)
    })
})
```


#### 第五题(加上nexttick)
```js
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})

```
### 第六题
```js
console.log(1)

setTimeout(() => {
    console.log(2)
    new Promise(resolve => {
        console.log(4)
        resolve()
    }).then(() => {
        console.log(5)
    })
    process.nextTick(() => {
        console.log(3)
    })
})

new Promise(resolve => {
    console.log(7)
    resolve()
}).then(() => {
    console.log(8)
})

process.nextTick(() => {
    console.log(6)
})
```
#### 第七题
```js
console.log(1)

setTimeout(() => {
    console.log(2)
    new Promise(resolve => {
        console.log(4)
        resolve()
    }).then(() => {
        console.log(5)
    })
    process.nextTick(() => {
        console.log(3)
    })
})

new Promise(resolve => {
    console.log(7)
    resolve()
}).then(() => {
    console.log(8)
})

process.nextTick(() => {
    console.log(6)
})

setTimeout(() => {
    console.log(9)
    process.nextTick(() => {
        console.log(10)
    })
    new Promise(resolve => {
        console.log(11)
        resolve()
    }).then(() => {
        console.log(12)
    })
})

```

## 实现
在第六第七题的时候,我们发现`nextTick`在8.X的执行跟我们预想的不是太一样,这是因为不同的引擎中的实现不是太一样,虽然chrome和node都基于v8引擎，但引擎只负责管理内存堆栈，API还是由各runtime自行设计并实现的。

### 第8题(小测试)
```js
setTimeout(() => {
	console.log(2)
}, 2)

setTimeout(() => {
	console.log(1)
}, 1)

setTimeout(() => {
	console.log(0)
}, 0)
```
如果对浏览器有一定了解的人可能会说出`2 1 0`这个输出因为MDN的setTimeout文档中提到HTML规范最低延时为4ms：
*（补充说明：最低延时的设置是为了给CPU留下休息时间）*

[实际延时比设定值更久的原因：最小延迟时间](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout#%E5%AE%9E%E9%99%85%E5%BB%B6%E6%97%B6%E6%AF%94%E8%AE%BE%E5%AE%9A%E5%80%BC%E6%9B%B4%E4%B9%85%E7%9A%84%E5%8E%9F%E5%9B%A0%EF%BC%9A%E6%9C%80%E5%B0%8F%E5%BB%B6%E8%BF%9F%E6%97%B6%E9%97%B4)
### Chrome中的timer
从`第8题`结果可以看出，0ms和1ms的延时效果是一致的，那背后的原因是为什么呢？我们先查查blink的实现。

（Blink代码托管的地方我都不知道如何进行搜索，还好文件名比较明显，没花太久，找到了答案）

*（我直接贴出最底层代码，上层代码如有兴趣请自行查阅）*
```c++
// https://chromium.googlesource.com/chromium/blink/+/master/Source/core/frame/DOMTimer.cpp#93

double intervalMilliseconds = std::max(oneMillisecond, interval * oneMillisecond); 
```
这里interval就是传入的数值，可以看出传入0和传入1结果都是oneMillisecond，即1ms。

这样解释了为何1ms和0ms行为是一致的，那4ms到底是怎么回事？我再次确认了HTML规范，发现虽然有4ms的限制，但是是存在条件的，详见规范第11点：

If nesting level is greater than 5, and timeout is less than 4, then set timeout to 4.
并且有意思的是，MDN英文文档的说明也已经贴合了这个规范。

我斗胆推测，一开始HTML5规范确实有定最低4ms的规范，不过在后续修订中进行了修改，我认为甚至不排除规范在向实现看齐，即逆向影响。

### Node中的timer
那node中，为什么0ms和1ms的延时效果一致呢？
```js

// https://github.com/nodejs/node/blob/v8.9.4/lib/timers.js#L456

if (!(after >= 1 && after <= TIMEOUT_MAX))
  after = 1; // schedule on next tick, follows browser behavior
```
代码中的注释直接说明了，设置最低1ms的行为是为了向浏览器行为看齐。

### Node中的Event Loop
- Node的Event Loop分阶段，阶段有先后，依次是
  - expired timers and intervals，即到期的setTimeout/setInterval
  - I/O events，包含文件，网络等等
  - immediates，通过setImmediate注册的函数
  - close handlers，close事件的回调，比如TCP连接断开


- 同步任务及每个阶段之后都会清空microtask队列
  - 优先清空next tick queue，即通过process.nextTick注册的函数
  - 再清空other queue，常见的如Promise
- **而和规范的区别，在于node会清空当前所处阶段的队列，即执行所有task**
node 在8.12版本之后就跟浏览器的行为保持一致了


## 微任务
如果在执行的微任务中再次注册微任务,会直接放到当前的微任务的执行栈中直接执行;详情参考第一题

## async await

### async

当我们在函数前使用async的时候，使得该函数返回的是一个Promise对象
```js
async function test() {
    return 1   // async的函数会在这里帮我们隐士使用Promise.resolve(1)
}
// 等价于下面的代码
function test() {
   return new Promise(function(resolve, reject) {
       resolve(1)
   })
}
```

### await
await表示等待，是右侧「表达式」的结果，这个表达式的计算结果可以是 Promise 对象的值或者一个函数的值（换句话说，就是没有特殊限定）。并且只能在带有async的内部使用

使用await时，会从右往左执行，当遇到await时，会阻塞函数内部处于它后面的代码，去执行该函数外部的同步代码，当外部同步代码执行完毕，再回到该函数内部执行剩余的代码, 并且当await执行完毕之后，会先处理微任务队列的代码

### 第九题
```js
setTimeout(function () {
  console.log('1')
}, 0)
console.log('2')
async function async1() {
  console.log('3')
  await async2()
  console.log('4')
}
async function async2() {
  console.log('5')
}
async1()
console.log('6')
```

### 第十题
```js
async function async1() {
  console.log('1')
  const data = await async2()
  console.log(data)
  console.log('2')
}

async function async2() {
  return new Promise(function (resolve) {
    console.log('3')
    resolve('await的结果')
  }).then(function (data) {
    console.log('4')
    return data
  })
}
console.log('5')

setTimeout(function () {
  console.log('6')
}, 0)

async1()

new Promise(function (resolve) {
  console.log('7')
  resolve()
}).then(function () {
  console.log('8')
})
console.log('9')
```
## 答案
### 第一题
```js
1
4
7
5
8
2
3
6

```
### 第三题
```js
8
2
6
3
4
5
7
1
```
### 第四题
```js
1
7
8
2
4
5
9
11
12
```

### 第五题

```js
1
7
6
8
2
4
3
5
9
11
10
12
```
### 第六题
```js
1
7
6
8
2
4
3
5
```

### 第七题
```js
1、7、6、8、2、4、3、5 9、11、10、12
//node 8.9 1、7、6、8、2、4、9、11、3、10、5、12
```

### 第八题
```js
// chrome: 1 0 2
// node(玄学) 1 0 2, 1 2 0,  2 1 0 ....
```

### 第九题

```js
2
3
5
6
4
1
```
### 第十题
```js
6
1
3
8
10
5
9
4
2
7

```
