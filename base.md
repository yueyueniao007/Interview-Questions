
## 1. 前端需要注意哪些SEO
- 合理的`title`、`description`、`keywords`：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；description把页面内容高度概括，长度合适，不可过分堆砌关键词，不同页面description有所不同；keywords列举出重要关键词即可
- 语义化的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页
- 重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取
- 重要内容不要用js输出：爬虫不会执行js获取内容
- 少用iframe：搜索引擎不会抓取iframe中的内容
- 非装饰性图片必须加alt
- 提高网站速度：网站速度是搜索引擎排序的一个重要指标

## 2.如何做SEO优化?
- 标题与关键词

	- 设置有吸引力切合实际的标题，标题中要包含所做的关键词

- 网站结构目录

	- 最好不要超过三级，每级有“面包屑导航”，使网站成树状结构分布

- 页面元素
 
	- 给图片标注"Alt"可以让搜索引擎更友好的收录

- 网站内容

	- 每个月每天有规律的更新网站的内容，会使搜索引擎更加喜欢

- 友情链接

	- 对方一定要是正规网站，每天有专业的团队或者个人维护更新

- 内链的布置

	- 使网站形成类似蜘蛛网的结构，不会出现单独连接的页面或链接

- 流量分析
	
	-通过统计工具(百度统计，CNZZ)分析流量来源，指导下一步的SEO
	
- content方面
	
	- 减少HTTP请求：合并文件、CSS精灵、inline Image
	- 减少DNS查询：DNS缓存、将资源分布到恰当数量的主机名
	- 减少DOM元素数量

- Server方面
	
	- 使用CDN
	- 配置ETag	
	- 对组件使用Gzip压缩

- Javascript方面
	
	- 将脚本放到页面底部
	- 将javascript和css从外部引入
	- 压缩javascript和css
	- 删除不需要的脚本
	- 减少DOM访问

- 图片方面
	
	- 优化图片：根据实际颜色需要选择色深、压缩
	- 优化css精灵
	- 不要在HTML中拉伸图片

## 3.http状态码有那些？分别代表是什么意思？
```
 简单版
    [
        100  Continue   继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
        200  OK         正常返回信息
        201  Created    请求成功并且服务器创建了新的资源
        202  Accepted   服务器已接受请求，但尚未处理
        301  Moved Permanently  请求的网页已永久移动到新位置。
        302 Found       临时性重定向。
        303 See Other   临时性重定向，且总是使用 GET 请求新的 URI。
        304  Not Modified 自从上次请求后，请求的网页未修改过。

        400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
        401 Unauthorized 请求未授权。
        403 Forbidden   禁止访问。
        404 Not Found   找不到如何与 URI 相匹配的资源。

        500 Internal Server Error  最常见的服务器端错误。
        503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
    ]
```

## 4.一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？（流程说的越详细越好）
- 注：这题胜在区分度高，知识点覆盖广，再不懂的人，也能答出几句，
- 而高手可以根据自己擅长的领域自由发挥，从URL规范、HTTP协议、DNS、CDN、数据库查询、
- 到浏览器流式解析、CSS规则构建、layout、paint、onload/domready、JS执行、JS API绑定等等；

- 详细版：
    - 浏览器会开启一个线程来处理这个请求，对 URL 分析判断如果是 http 协议就按照 Web 方式来处理;
    - 调用浏览器内核中的对应方法，比如 WebView 中的 loadUrl 方法;
    - 通过DNS解析获取网址的IP地址，设置 UA 等信息发出第二个GET请求;
    - 进行HTTP协议会话，客户端发送报头(请求报头);
    - 进入到web服务器上的 Web Server，如 Apache、Tomcat、Node.JS 等服务器;
    - 进入部署好的后端应用，如 PHP、Java、JavaScript、Python 等，找到对应的请求处理;
    - 处理结束回馈报头，此处如果浏览器访问过，缓存上有对应资源，会与服务器最后修改时间对比，一致则返回304;
    - 浏览器开始下载html文档(响应报头，状态码200)，同时使用缓存;
    - 文档树建立，根据标记请求所需指定MIME类型的文件（比如css、js）,同时设置了cookie;
    - 页面开始渲染DOM，JS根据DOM API操作DOM,执行事件绑定等，页面显示完成。

- 简洁版：
    - 浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
    - 服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
    - 浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
    - 载入解析到的资源文件，渲染页面，完成。

## 5.常见排序算法的时间复杂度,空间复杂度
![排序算法比较](img/sort-compare.png)

## 6.web 开发中会话跟踪的方法有哪些
- cookie
- session
- url 重写
- 隐藏 input
- ip 地址

## 7.`<img>`的`title`和`alt`有什么区别
- `title`是[global attributes](http://www.w3.org/TR/html-markup/global-attributes.html#common.attrs.core)之一，用于为元素提供附加的 advisory information。通常当鼠标滑动到元素上的时候显示。
- `alt`是`<img>`的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析。

## 8.doctype 是什么,举例常见 doctype 及特点

- `<!doctype>`声明必须处于 HTML 文档的头部，在`<html>`标签之前，HTML5 中不区分大小写
- `<!doctype>`声明不是一个 HTML 标签，是一个用于告诉浏览器当前 HTMl 版本的指令
- 现代浏览器的 html 布局引擎通过检查 doctype 决定使用兼容模式还是标准模式对文档进行渲染，一些浏览器有一个接近标准模型。
- 在 HTML4.01 中`<!doctype>`声明指向一个 DTD，由于 HTML4.01 基于 SGML，所以 DTD 指定了标记规则以保证浏览器正确渲染内容
- HTML5 不基于 SGML，所以不用指定 DTD

	## 常见 dotype：

- **HTML4.01 strict**：不允许使用表现性、废弃元素（如 font）以及 frameset。声明：`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`
- **HTML4.01 Transitional**:允许使用表现性、废弃元素（如 font），不允许使用 frameset。声明：`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">`
- **HTML4.01 Frameset**:允许表现性元素，废气元素以及 frameset。声明：`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">`
- **XHTML1.0 Strict**:不使用允许表现性、废弃元素以及 frameset。文档必须是结构良好的 XML 文档。声明：`<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">`
- **XHTML1.0 Transitional**:允许使用表现性、废弃元素，不允许 frameset，文档必须是结构良好的 XMl 文档。声明： `<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">`
- **XHTML 1.0 Frameset**:允许使用表现性、废弃元素以及 frameset，文档必须是结构良好的 XML 文档。声明：`<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">`
- **HTML 5**: `<!doctype html>`

## 9.HTML 全局属性(global attribute)有哪些

参考资料：[MDN: html global attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)或者[W3C HTML global-attributes](http://www.w3.org/TR/html-markup/global-attributes.html#common.attrs.core)

- `accesskey`:设置快捷键，提供快速访问元素如<a href="#" accesskey="a">aaa</a>在 windows 下的 firefox 中按`alt + shift + a`可激活元素
- `class`:为元素设置类标识，多个类名用空格分开，CSS 和 javascript 可通过 class 属性获取元素
- `contenteditable`: 指定元素内容是否可编辑
- `contextmenu`: 自定义鼠标右键弹出菜单内容
- `data-*`: 为元素增加自定义属性
- `dir`: 设置元素文本方向
- `draggable`: 设置元素是否可拖拽
- `dropzone`: 设置元素拖放类型： copy, move, link
- `hidden`: 表示一个元素是否与文档。样式上会导致元素不显示，但是不能用这个属性实现样式效果
- `id`: 元素 id，文档内唯一
- `lang`: 元素内容的的语言
- `spellcheck`: 是否启动拼写和语法检查
- `style`: 行内 css 样式
- `tabindex`: 设置元素可以获得焦点，通过 tab 可以导航
- `title`: 元素相关的建议信息
- `translate`: 元素和子孙节点内容是否需要本地化
- 123
- 123