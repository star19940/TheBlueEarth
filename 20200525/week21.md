#### 一、promise

简介

一个容器，包含着某个未来才会结束的事件

resolve：当前promise从pending状态转成完成状态，执行reslove方法；将异步操作的结果当作参数传递出去；

reject：当前promise从pending状态转成失败状态，执行reject方法；将失败的原因传递出去；

Promise是一个构造函数，new Promise()实例化这个构造函数；

工作原理



###### 哪些api使用了

fetch请求

###### 新建promise

promise传入的参数是两个方法，为resolve方法、reject方法；其中第二个参数reject方法可以省略。这两个方法是js自动生成，不用定义

###### 错误处理（级联错误处理）

利用catch去获取

级联时候再次使用catch捕获

###### 链式调用

.then().then()...

fetch请求在使用的时候就用了链式调用，利用then获取返回状态->返回数据json处理->处理数据

##### promise.all()

all中传入一个数组，最后完全解析后再执行。all方法的效果实际上是「谁跑的慢，以谁为准执行回调时间」

**race的用法**

all方法的效果实际上是「谁跑的慢，以谁为准执行回调」，那么相对的就有另一个方法「谁跑的快，以谁为准执行回调时间」，这就是race方法

##### 经典题目

```
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
```

解释：

new Promise是立即执行所以优先打印// Promise

.then()方法是在当前脚本执行完后执行，所以会先打印// Hi! ，最后打印// resolved

**二、什么是 `async/await` 及其如何工作？**

参考链接：https://juejin.im/post/5c553b71f265da2d8532b351

###### async/await 是Generator的语法糖；

Generator是异步操作，利用*来定义，语句用yeild来控制，执行完当前yeild才执行下一句，每一句yeild都有value和done属性，其中done属性true和false，当为true时表示当前所有语句全部执行完。

举例2: 平常当处理一个文件时，分为读取文件和处理文件两步，都是读取文件后再处理文件；那么用了Generator 后，就可以在读取文件后，处理文件前，插入你想要执行的程序，等待执行完之后在执行处理文件这一步，就达到了异步的 效果。

###### async/await 和 Generator 不同点：

返回结果以promise封装，可以执行then函数；

函数定义名称更加语义化；

async/await 使用

```
async function asyncFn3 () {
    return await Promise.resolve('hello async')
}

asyncFn3().then(res => console.log(res))
// 'hello async'
```

async/await 与 promise 对比：

```
function geta (provinceId) {
    ...
    return new Promise(resolve => {
        resolve(a)
    }
}
geta().then((a) => {getb(a)}).then((b) => {getb(b)})

async function  get() {
    ...
    await b = geta(a);
    await c = getb(b);
    return await getc(c);
}
```

执行过程：

包含settimeout，newPromise，then()

```
async function asyncFn1 () {
    console.log('asyncFn1 start')//2
    await asyncFn2()
    console.log('async1 end')//6
}

async function asyncFn2 () {
    console.log('asyncFn2')//3
}

console.log('script start')//1

setTimeout(function () {
    console.log('setTimeout')//8
}, 0)

asyncFn1()

new Promise((resolve) => {
    console.log('Promise')//4
    resolve()
}).then(() => {
    console.log('Promise.then')//7
})
console.log('script end')//5

```

###### 执行顺序注意点：

await 所代表的点是插入执行，执行完后继续执行当前同步的脚本。

当前同步脚本执行完后再执行then()

setTimeout是所有同步异步最后执行的，即使事件间隔是0。

宏任务，微任务？？？？

##### 疑问点：

当前同步脚本执行完后，then()与await的下一句脚本执行顺序有待考究？？也就是6、7的执行顺序。

