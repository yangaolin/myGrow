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

