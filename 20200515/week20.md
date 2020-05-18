**undefined 和 null 有什么区别？**

1. null就表示没有这个属性

   原型链的结束

2. undefined 表示定义了但是没有被赋值，变量、对象、函数返回值未定义、函数未传必传的参数

3. ```
   var a = null;
   typeOf a;// Object
   
   var a;
   typeOf a;//undefined
   ```

   

**什么是事件冒泡、事件传播、事件捕获？**

明确概念点：事件、事件流，dom二级事件流，事件冒泡、事件捕获、事件委托

ie与网景提出的不同概念，ie提倡事件冒泡，就是自下（当前div）而上（document），网景提倡事件捕获，就是自上（document）而下（当前div）。

事件流包括事件捕获-执行阶段-事件冒泡，现在主流浏览器都是时间冒泡。

w3C进行统一，dom二级事件流包括事件捕获-执行阶段-事件冒泡在addEventListener中的第三个参数true和false来进行控制，默认是false也就是利用冒泡处理。

事件委托(事件代理)：就是利用事件冒泡原理，将事件放在父级上，减少内存占用，实现动态绑定。

###### 原理

事件委托是利用事件冒泡原理，事件冒泡意思是比如当前dom结构为div>ul>li>a，那么a上面有click事件时，就会冒泡到div上，顺序为a>li>ul>div。那么就会有这样一个机制，在div上绑定click事件时，通过点击a，那么会冒泡到li、ul、div上，这个就是事件委托机制，委托父级为子级执行事件。

常用：

当我们要给li标签增加click事件时，通常循环找到每一个li，然后给他们增加click事件。利用事件委托，就可以将事件添加到ul上，点击li的时候会冒泡到ul，通过ev.target（标准浏览器，ie为ev.srcElement）来获取当前li。

其他场景，不同的li添加click事件但是执行方法不同？动态增加li节点？

https://www.jianshu.com/p/ef18c1afa339

提出问题：

addEventListener、on、bind、onClick、each

addEventListener 主流浏览器、在ie8以上版本使用（可以事件增加，不会移除）

attchEvent ie8及更早版本