## 1. javaScript的组成
- ECMAScript(核心)：javaScript语言基础
- DOM(文档对象模型)：规定了访问HTML和XML的接口
- BOM(浏览器对象模型)：提供了浏览器窗口之间进行交互的对象和方法

## 2. javaScript的基本数据类型和引用数据类型
- 基本数据类型：undefined，null，boolean，number，string，symbol
- 引用数据类型：object，array，function

## 3. 检测浏览器版本版本有哪些方式？
- 根据 navigator.userAgent   //  UA.toLowerCase().indexOf('chrome')
- 根据 window 对象的成员       // 'ActiveXObject' in window

## 4. 介绍js有哪些内置对象
- 数据封装类对象：Object、Array、Boolean、Number、String
- 其他对象：Function、Arguments、Math、Date、RegExp、Error
- ES6新增对象：Symbol、Map、Set、Promises、Proxy、Reflect

## 5. 描述浏览器的渲染过程，DOM树和渲染树的区别
- 浏览器的渲染过程：
    - 解析HTML构建 DOM(DOM树)，并行请求 css/image/js
    - CSS 文件下载完成，开始构建 CSSOM(CSS树)
    - CSSOM 构建结束后，和 DOM 一起生成 Render Tree(渲染树)
    - 布局(Layout)：计算出每个节点在屏幕中的位置
    - 显示(Painting)：通过显卡把页面画到屏幕上

- DOM树 和 渲染树 的区别：
    - DOM树与HTML标签一一对应，包括head和隐藏元素
    - 渲染树不包括head和隐藏元素，大段文本的每一个行都是独立节点，每一个节点都有对应的css属性

## 6. 重绘和回流（重排）的区别和关系？
- 重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
- 回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
- 注意：JS获取Layout属性值（如：offsetLeft、scrollTop、getComputedStyle等）也会引起回流。因为浏览器需要通过回流计算最新值
- 回流必将引起重绘，而重绘不一定会引起回流

## 7. 如何最小化重绘(repaint)和回流(reflow)？
- 需要要对元素进行复杂的操作时，可以先隐藏(display:"none")，操作完成后再显示
- 需要创建多个DOM节点时，使用DocumentFragment创建完后一次性的加入document
- 缓存Layout属性值，如：var left = elem.offsetLeft; 这样，多次使用 left 只产生一次回流
- 尽量避免用table布局（table元素一旦触发回流就会导致table里所有的其它元素回流）
- 避免使用css表达式(expression)，因为每次调用都会重新计算值（包括加载页面）
- 尽量使用 css 属性简写，如：用 border 代替 border-width, border-style, border-color
- 批量修改元素样式：elem.className 和 elem.style.cssText 代替 elem.style.xxx

## 8.类型判断

```
'' == 0            //true
'   ' == 0         //true
null == undefined  //true
null == 0          //false
undefined == ''    //false
'false' == false   //false
NaN == NaN         //false
NaN == false       //false
NaN === false      //false


var a = {};
var b = {};
var c = a;

//引用类型比较的是地址

a == b;            //false
a === b;           //false
a == c;            //true
a === c;           //true
```

## 9.防抖和节流
### 防抖：就是一定时间内，只会执行最后一次任务;
### 节流：就是一定时间内，只执行一次;

#### 防抖

```
function debounce() {
	let timer;
	return function () {
		clearTimeout(timer);
		timer = setTimeout(() => {
			console.log("防抖成功！");
		}, 500);
	}
}
```

#### 节流

```
function throttle() {
	let flag = true;
	return function () {
		if (!flag) { return; }
		flag = false;
		setTimeout(() => {
			console.log("节流成功！");
			flag = true;
		}, 1000);
	};
}
```


11111