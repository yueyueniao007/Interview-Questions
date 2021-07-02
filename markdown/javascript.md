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
- 
## 5. 描述浏览器的渲染过程，DOM树和渲染数的区别
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







