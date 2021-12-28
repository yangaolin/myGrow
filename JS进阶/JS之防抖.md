**前言**
--- 
在前端开发中会遇到一些频繁的事件触发，比如：
1. window 的 resize、scroll
2. mousedown、mousemove
3. keyup、keydown
……
为此，我们举个示例代码来了解事件如何频繁的触发：

我们写个 `index.html` 文件：
```
<!DOCTYPE html>
<html lang="zh-cmn-Hans">

<head>
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="IE=edge, chrome=1">
    <title>debounce</title>
    <style>
        #container{
            width: 100%; height: 200px; line-height: 200px; text-align: center; color: #fff; background-color: #444; font-size: 30px;
        }
    </style>
</head>

<body>
    <div id="container"></div>
    <script src="debounce.js"></script>
</body>

</html>

```

`debounce.js` 文件的代码如下：

```
var count = 1;
var container = document.getElementById('container');

function getUserAction() {
    container.innerHTML = count++;
};

container.onmousemove = getUserAction;

```

我们来看看效果：

![1](https://user-images.githubusercontent.com/19302489/147581034-6327579b-3ece-4ef4-aaff-5ab8ebec5e88.gif)

从左边滑到右边就触发了 165 次 getUserAction 函数！

因为这个例子很简单，所以浏览器完全反应的过来，可是如果是复杂的回调函数或是 ajax 请求呢？假设 1 秒触发了 60 次，每个回调就必须在 1000 / 60 = 16.67ms 内完成，否则就会有卡顿出现。

为了解决这个问题，一般有两种解决方案：

1. debounce 防抖
2. throttle 节流

**防抖**
--- 

今天重点讲讲防抖的实现。

防抖的原理就是：你尽管触发事件，但是我一定在事件触发 n 秒后才执行，如果你在一个事件触发的 n 秒内又触发了这个事件，那我就以新的事件的时间为准，n 秒后才执行，总之，就是要等你触发完事件 n 秒内不再触发事件，我才执行，真是任性呐!

**第一版**
---
根据这段表述，我们可以写第一版的代码：

```
// 第一版
function debounce(func, wait) {
    var timeout;
    return function () {
        clearTimeout(timeout)
        timeout = setTimeout(func, wait);
    }
}

```
如果我们要使用它，以最一开始的例子为例：

```
container.onmousemove = debounce(getUserAction, 1000);

```
现在随你怎么移动，反正你移动完 1000ms 内不再触发，我才执行事件。看看使用效果：

![2](https://user-images.githubusercontent.com/19302489/147581958-b696967a-a438-416f-978b-e8b57f5b7569.gif)

顿时就从 165 次降低成了 1 次!

棒棒哒，我们接着完善它。




