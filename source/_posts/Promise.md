---
title: Promise
catalog: true
date: 2020-11-16 23:14:31
subtitle:
header-img:
tags:
- JavaScript
- FrontEnd
---
<center>Promise</center>

>一个 Promise 对象代表一个在这个 promise 被创建出来时不一定已知的值。它让您能够把异步操作最终的成功返回值或者失败原因和相应的处理程序关联起来。 这样使得异步方法可以像同步方法那样返回值：异步方法并不会立即返回最终的值，而是会返回一个 promise，以便在未来某个时候把值交给使用者.--developer.mozilla.org

>简言之: 用来处理异步操作的对象.


# Promise的状态.

* 待定(pending): 初始状态.
    >一个Promise被创建,还没有 resolve或者reject.

* 已兑现: resolve操作后, 表示操作完成成功.

* 已拒绝: reject操作后,表示操作失败.

# Promise的使用.

* 注意, Promise如果状态一旦被确定为 兑现(resolve后),或者拒绝(reject后).就算完成了整个流程.后面不管有其他任何操作,都不会改变Promise的状态.

```JavaScript

// 一般使用场景
var p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('done')
    }, 3000);
});

p.then(msg => {
    console.log('msg:' + msg);
});
// 运行结果: 3秒后.  msg:done

```

## Promise的使用,需要注意以下几点.

1. resolve或者reject后,其后面的代码,还是会运行的!

```JavaScript
// eg:
var p = new Promise((resolve) => {
    resolve('done');
    console.log('after done');
});
p.then(msg => {
    console.log('msg:' + msg);
});
// 结果:
// after done
// msg:done

// 注意, 但凡是 Promise都是异步,所以,then后面的会在 'after done'后面.
```

2. Promise的错误捕获,有两种写法.

```JavaScript
// eg:
var p = new Promise((resolve, reject) => {
    reject('error');
});

// 写法1. 适合单个Promise时.
p.then((msg) => {
    console.log('msg:' + msg);
}, error => {
    console.error(error);
});

// 写法2. 这种写法,适合多个链式调用,放在链式调用的末尾.
p.then((msg) => {
    console.log('msg:' + msg);
}).catch(error => {
    console.error(error);
});
```

3. 对一个Promise,多次用then接受时, 不会多次执行内部代码.

```JavaScript
// Promise状态敲定后, 不会再改变. 就算是多次调用　.then 也会是一样的.
// eg:
var count = 0;
var p = new Promise((resolve, reject) => {
    count ++;
    resolve(count);
});

p.then((msg) => {
    console.log('then1: ' + msg);
});
p.then((msg) => {
    console.log('then2: ' + msg);
});

// 结果: 
// then1: 1
// then2: 1
```

# Promise的一些静态方法.

* Promise.all, 传入一个Promise对象的数组,所有成功时, 才接收成功,一旦有一个是失败,那么就是失败.

```JavaScript
function sleep(time) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(time)
        }, time);
    })
}

Promise.all([
    sleep(100),
    sleep(300),
    sleep(1000)
]).then(result => {
    console.log(result);
});
// 结果: 1秒后.
// [100, 300, 1000]

```

* Promise.race 接收一个 Promise的对象的数组,当其中任何一个 Promise成功了或者失败了,返回最先执行完的那个.

```JavaScript
function sleep(time, flag) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(time);
            console.log('flag:' + flag);
        }, time);
    })
}

Promise.race([
    sleep(100, '1'),
    sleep(300, '2'),
    sleep(1000, '3')
]).then(result => {
    console.log(result);
});
// 结果:
// flag:1
// 100
// flag:2
// flag:3

// 注意: 虽然其他的Promise不会被返回,但是,其内部的代码,是会执行的.

// 这里, 100 会在 flag之后, 是因为, Promise作为一个异步,已经排在异步级别了(类似于 setTimeout 0). 后面的 setTimeout,会排在他后面

// 如果把 sleep中的setTimeout去掉, 就会是  
// flag:1
// flag:2
// flag:3
// 100.
```

