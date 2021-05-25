
## 1. css sprite是什么，有什么优缺点？
**概念：**将多个小图片拼接到一个图片中。通过background-position和元素尺寸调节需要显示的背景图案

**优点：**

- 减少http请求数，极大的提高页面加载速度
- 增加图片信息重复度，提高压缩比，减小图片大小
- 更换风格方便，只需在一张或几张图片上修改颜色或者样式即可

**缺点：**

- 图片合成麻烦
- 维护麻烦，修改一个图片可能需要重新布局整个图片，样式

## 2.`display:none;`和`visibility:hidden;`的区别？

**联系：**他们都能让元素不可见

**区别：**

- `display:none;`会让元素完全从渲染树中消失，渲染的时候不占据任何空间；`visibility:hidden;`不会让元素从渲染树消失，渲染时元素继续占据空间，只是内容不可见。
- `display:none;`是非继承属性，子孙节点消失是由于父元素从渲染树消失所造成的，通过修改子孙节点属性无法显示；`visibility:hidden;`是继承属性，子孙节点由于继承了hidden而消失，通过设置`visibility:visible;`可以让子孙节点显示。
- 修改常规流中元素的dsiplay通常会造成元素重排。修改visibility属性只会造成本元素重绘。
- 读屏器不会读取`display:none;`元素内容；会读取`visibility:hidden;`内容。

## 3.css hack原理

**原理：**利用不同浏览器对CSS的支持和解析结果不一样编写针对特定浏览器样式。

- 常见的hack有
  - 属性hack
  - 选择器hack
  - IE条件注释

## 4.`link`与`@important`的区别

- `link`是HTML方式，`@important`是css方式
- `link`最大限度支持并行下载，`@important`过多嵌套会导致串行下载，出现 [FOUC]()
- `link`可以通过`rel="alternate stylesheet"`指定候选样式
- 浏览器对`link`支持早于`@important`，可以使用`@important`对老浏览器隐藏样式
- `@important`必须在样式规则之前，可以在css文件中引入其它文件
- 总体来说：**[link 优于@import](https://blog.csdn.net/weixin_42441117/article/details/80705153)**

**FOUC：**由于css引入使用了@import 或者存在多个style标签以及css文件在页面底部引入使得css文件加载在html之后导致页面闪烁、花屏

## 5.`display:block;`与`display:inline;`的区别

**block元素特点**

- 处于常规流中时，如果`width`没有设置，会自动填充满父元素
- 可以应用`margin/padding`
- 在没有设置高度的情况下会扩展高度以包含常规流中的子元素
- 处于常规流中时布局时在前后元素位置之间（独占一个水平空间）
- 忽略`vertical-align`

**inline元素特点**

- 水平方向上根据`direction`依次布局
- 不会在元素前后进行换行
- 受`white-space`控制
- `margin/padding`在竖直方向上无效，水平方向上有效
- `width/height`属性对非替换行内元素无效，宽度由元素内容决定
- 非替换行内元素的行框高由`line-height`确定，替换行内元素的行框高由`height`,`margin`,`padding`,`border`决定
- 浮动或绝对定位时会替换为`block`
- `vertical-align`无效