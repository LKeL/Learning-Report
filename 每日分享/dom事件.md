# dom事件

首先既然是web标准，那么就亮出官方标准草案 [传送门](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)

主要有两个问题

- 什么是事件流
- 什么是事件流的三个阶段

## 流

流可以认为是一种有方向的数据，是对输入输出的抽象


```javascript
element.addEventListener(<event-name>, <callback>, <use-capture>);
element.removeEventListener(<event-name>, <callback>, <use-capture>);
```

## 三个阶段

- 事件捕获阶段
- 处于目标阶段
- 事件冒泡阶段


当事件发生时，首先发生的是事件捕获，为父元素截获事件提供了机会

```javascript
window.addEventListener('click', function() {
  console.log('capture');
}, true);
```

当事件到达目标节点的，事件就进入了目标阶段，一般被看成冒泡阶段的一部分。事件在目标节点上被触发，然后会逆向回流，直到传播至最外层的文档节点

事件冒泡过程，是可以被阻止的`stopPropagation`


优先使用事件冒泡，特殊情况使用事件捕获

注意维护事件上下文，我倾向于使用`bind`来绑定


## Event对象

Event对象在event第一次触发的时候被创建出来，并且一直伴随着事件在DOM结构中流转的整个生命周期。event对象会被作为第一个参数传递给事件监听的回调函数。我们可以通过这个event对象来获取到大量当前事件相关的信息：

- `type (String)` — 事件的名称
- `target (node)` — 事件起源的DOM节点
- `currentTarget?(node)` — 当前回调函数被触发的DOM节点
- `bubbles (boolean)` — 指明这个事件是否是一个冒泡事件
- `preventDefault(function)` — 这个方法将阻止浏览器中用户代理对当前事件的相关默认行为被触发。比如阻止`<a>`元素的click事件加载一个新的页面
- `stopPropagation (function)` — 这个方法将阻止当前事件链上后面的元素的回调函数被触发，当前节点上针对此事件的其他回调函数依然会被触发。
- `stopImmediatePropagation (function)` — 这个方法将阻止当前事件链上所有的回调函数被触发，也包括当前节点上针对此事件已绑定的其他回调函数。
- `cancelable (boolean)` — 这个变量指明这个事件的默认行为是否可以通过调用`event.preventDefault`来阻止。也就是说，只有`cancelable`为`true`的时候，调用`event.preventDefault`才能生效。
- `defaultPrevented (boolean)` — 这个状态变量表明当前事件对象的`preventDefault`方法是否被调用过
- `isTrusted (boolean)` — 如果一个事件是由设备本身（如浏览器）触发的，而不是通过`JavaScript`模拟合成的，那个这个事件被称为可信任的(trusted)
- `eventPhase (number)` — 这个数字变量表示当前这个事件所处的阶段`(phase):none(0), capture(1),target(2),bubbling(3)`。我们会在下一个部分介绍事件的各个阶段
- `timestamp (number)` — 事件发生的时间


## 一些有用的事件

- load事件可以在任何资源（包括被依赖的资源）被加载完成时被触发，这些资源可以是图片，css，脚本，视频，音频等文件，也可以是document或者window。
- window.onbeforeunload让开发人员可以在想用户离开一个页面的时候进行确认。这个在有些应用中非常有用，比如用户不小心关闭浏览器的tab，我们可以要求用户保存他的修改和数据，否则将会丢失他这次的操作
- 移动端event.preventDefault阻止窗口抖动
- resize来监听并应用响应式布局
- transitionend特定动画的结束时间
- @keyframe用animationEnd
- animationiteration事件会在当前的动画元素完成一个动画迭代的时候被触发
- error，不必多说


事件模型的成功上，我们可以学到很多。我们可以在我们的项目中使用类似的解耦的概念。应用中的模块可以有很高的很复杂度，只要它的复杂度被封装隐藏在一套简单的接口背后

比如使用发布-订阅（publish and subscribe）的方式来处理跨模块间的通信


