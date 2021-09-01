
## 1 手写Promise系列

[《从小白视角上手Promise、Async/Await和手撕代码](https://mp.weixin.qq.com/s?__biz=MzU2Mjg0NDY5Ng==&mid=2247484686&idx=1&sn=a4e46712b8d688d827f5d63fd50991bb&chksm=fc620079cb15896fa1885b9040c5abec1c8425cdf20f562092e58bc44062a9b36d53702040e0&token=1964890954&lang=zh_CN&scene=21#wechat_redirect)》。

#### 1.1 Promise.all

```
//手写promise.all  
Promise.prototype._all = promiseList => {  
  // 当输入的是一个promise列表  
  const len = promiseList.length;  
  const result = [];  
  let count = 0;  
  //   
  return new Promise((resolve,reject)=>{  
    // 循环遍历promise列表中的promise事件  
    for(let i = 0; i < len; i++){  
      // 遍历到第i个promise事件，判断其事件是成功还是失败  
      promiseList[i].then(data=>{  
        result[i] = data;  
        count++;  
        // 当遍历到最后一个promise时，结果的数组长度和promise列表长度一致，说明成功  
        count === len && resolve(result);  
      },error=>{  
        return reject(error);  
      })  
    }  
  })  
}  
```

#### 1.2 Promise.race

```
// 手写promise.race  
Promise.prototype._race = promiseList => {  
  const len = promiseList.length;  
  return new Promise((resolve,reject)=>{  
    // 循环遍历promise列表中的promise事件  
    for(let i = 0; i < len; i++){  
      promiseList[i]().then(data=>{  
        return resolve(data);  
      },error=>{  
        return reject(error);  
      })  
    }  
  })  
}  
```

#### 1.3 Promise.finally

```
Promise.prototype._finally = function(promiseFunc){  
  return this.then(data=>Promise.resolve(promiseFunc()).then(data=>data)  
  ,error=>Promise.reject(promiseFunc()).then(error=>{throw error}))  
}  
```

## 2 手写Aysnc/Await

```
function asyncGenertor(genFunc){  
  return new Promise((resolve,reject)=>{  
    // 生成一个迭代器  
    const gen = genFunc();  
    const step = (type,args)=>{  
      let next;  
      try{  
        next = gen[type](args);  
      }catch(e){  
        return reject(e);  
      }  
      // 从next中获取done和value的值  
      const {done,value} = next;  
      // 如果迭代器的状态是true  
      if(done) return resolve(value);  
      Promise.resolve(value).then(  
        val=>step("next",val),  
        err=>step("throw",err)  
      )  
    }  
    step("next");  
  })  
}  
```

## 3 深拷贝
-----

**深拷贝：拷贝所有的属性值，以及属性地址指向的值的内存空间。**

#### 3.1 丢失引用的深拷贝

当遇到对象时，就再新开一个对象，然后将第二层源对象的属性值，完整地拷贝到这个新开的对象中。

```
// 丢失引用的深拷贝  
function deepClone(obj){  
  // 判断obj的类型是否为object类型  
  if(!obj && typeof obj !== "object") return;  
  // 判断对象是数组类型还是对象类型  
  let newObj = Array.isArray(obj) ? [] : {};  
  // 遍历obj的键值对  
  for(const [key,value] of Object.entries(obj)){  
    newObj[key] = typeof value === "string" ? deepClone(value) : value;  
  };  
  return newObj;  
}  
```

#### 3.2 终极方案的深拷贝（栈和深度优先的思想）

其思路是：引入一个数组 `uniqueList` 用来存储已经拷贝的数组，每次循环遍历时，先判断对象是否在 `uniqueList` 中了，如果在的话就不执行拷贝逻辑了。

```
function deepCopy(obj){  
  // 用于去重  
  const uniqueList = [];  
  // 设置根节点  
  let root = {};  
  // 遍历数组  
  const loopList = [{  
    parent: root,  
    key: undefined,  
    data: obj  
  }];  
  // 遍历循环  
  while(loopList.length){  
    // 深度优先-将数组最后的元素取出  
    const {parent,key,data} = loopList.pop();  
    // 初始化赋值目标，key--undefined时拷贝到父元素，否则拷贝到子元素  
    let result = parent;  
    if(typeof key !== "undefined") result = parent[key] = {};  
    // 数据已存在时  
    let uniqueData = uniqueList.find(item=>item.source === data);  
    if(uniqueData){  
      parent[key] = uniqueData.target;  
      // 中断本次循环  
      continue;  
    }  
    // 数据不存在时  
    // 保存源数据，在拷贝数据中对应的引用  
    uniqueList.push({  
      source:data,  
      target:result  
    });  
    // 遍历数据  
    for(let k in data){  
      if(data.hasOwnProperty(k)){  
        typeof data[k] === "object"   
        ?  
          // 下一次循环  
          loopList.push({  
            parent:result,  
            key:k,  
            data:data[k]  
          })  
        :   
        result[k] = data[k];  
          
      }  
    }  
  }  
  return root;  
}  
```

## 4 手写一个单例模式
----------

单例模式：保证一个类仅有一个实例，并提供一个访问它的全局访问点。实现方法一般是先判断实例是否存在，如果存在直接返回，如果不存在就先创建再返回。

```
// 创建单例对象，使用闭包  
const getSingle = function(func){  
  let result;  
  return function(){  
    return result || (result = func.apply(this,arguments));  
  }  
}  
  
// 使用Proxy拦截  
const proxy = function(func){  
  let reuslt;  
  const handler = {  
    construct:function(){  
      if(!result) result = Reflect.construct(func,arguments);  
      return result;  
    }  
  }  
  return new Proxy(func,hendler);  
}  
```

## 5 手写封装一个ajax函数
--------------

```
/*   
封装自己的ajax函数  
参数1：{string} method 请求方法  
参数2：{string} url 请求地址  
参数2：{Object} params 请求参数  
参数3：{function} done 请求完成后执行的回调函数  
*/  
  
function ajax(method,url,params,done){  
  // 1.创建xhr对象，兼容写法  
  let xhr = window.XMLHttpRequest   
  ? new XMLHttpRequest()  
  : new ActiveXObject("Microsoft.XMLHTTP");  
  
  // 将method转换成大写  
  method = method.toUpperCase();  
  // 参数拼接  
  let newParams = [];  
  for(let key in params){  
    newParams.push(`${key}=${params[k]}`);  
  }  
  let str = newParams.join("&");  
  // 判断请求方法  
  if(method === "GET") url += `?${str}`;  
  
  // 打开请求方式  
  xhr.open(method,url);  
  
  let data = null;  
  if(method === "POST"){  
    // 设置请求头  
    xhr.setRequestHeader(("Content-Type","application/x-www-form-urlencoded"));  
    data = str;  
  }  
  xhr.send(data);  
  
  // 指定xhr状态变化事件处理函数  
  // 执行回调函数  
  xhr.onreadystatechange = function(){  
    if(this.readyState === 4) done(JSON.parse(xhr.responseText));  
  }  
}  
```

## 6 手写“防抖”和“节流”
-------------

[一网打尽──他们都在用这些”防抖“和”节流“方法](http://mp.weixin.qq.com/s?__biz=MzU2Mjg0NDY5Ng==&mid=2247484071&idx=1&sn=3acef0bf025ff97bbd2810a79a52e77b&chksm=fc6207d0cb158ec694d3a6c96a801e1b4fdb15a14c6a9ce645ce9d49d7774f95483166cca6ca&scene=21#wechat_redirect)》。

#### 6.1 防抖

```
/*   
func：要进行防抖处理的函数  
delay：要进行延时的时间  
immediate：是否使用立即执行 true立即执行 false非立即执行  
*/  
function debounce(func,delay,immediate){  
  let timeout; //定时器  
  return function(arguments){  
    // 判断定时器是否存在，存在的话进行清除，重新进行定时器计数  
    if(timeout) clearTimeout(timeout);  
    // 判断是立即执行的防抖还是非立即执行的防抖  
    if(immediate){//立即执行  
      const flag = !timeout;//此处是取反操作  
      timeout = setTimeout(()=>{  
        timeout = null;  
      },delay);  
      // 触发事件后函数会立即执行，然后 n 秒内不触发事件才能继续执行函数的效果。  
      if(flag) func.call(this,arguments);  
    }else{//非立即执行  
      timeout = setTimeout(()=>{  
        func.call(this,arguments);  
      },delay)  
    }  
  
  }  
}  
  
```

#### 6.2 节流

```
 // 节流--定时器版  
  function throttle(func,delay){  
    let timeout;//定义一个定时器标记  
    return function(arguments){  
      // 判断是否存在定时器  
      if(!timeout){   
        // 创建一个定时器  
        timeout = setTimeout(()=>{  
          // delay时间间隔清空定时器  
          clearTimeout(timeout);  
          func.call(this,arguments);  
        },delay)  
      }  
    }  
  }  
```

## 7 手写apply、bind、call
-------------------

#### 7.1 apply

*   传递给函数的参数处理，不太一样，其他部分跟call一样。
    
*   apply接受第二个参数为类数组对象, 这里用了《JavaScript权威指南》中判断是否为类数组对象的方法。
    

```
Function.prototype._apply = function (context) {  
    if (context === null || context === undefined) {  
        context = window // 指定为 null 和 undefined 的 this 值会自动指向全局对象(浏览器中为window)  
    } else {  
        context = Object(context) // 值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的实例对象  
    }  
    // JavaScript权威指南判断是否为类数组对象  
    function isArrayLike(o) {  
        if (o &&                                    // o不是null、undefined等  
            typeof o === 'object' &&                // o是对象  
            isFinite(o.length) &&                   // o.length是有限数值  
            o.length >= 0 &&                        // o.length为非负值  
            o.length === Math.floor(o.length) &&    // o.length是整数  
            o.length < 4294967296)                  // o.length < 2^32  
            return true  
        else  
            return false  
    }  
    const specialPrototype = Symbol('特殊属性Symbol') // 用于临时储存函数  
    context[specialPrototype] = this; // 隐式绑定this指向到context上  
    let args = arguments[1]; // 获取参数数组  
    let result  
    // 处理传进来的第二个参数  
    if (args) {  
        // 是否传递第二个参数  
        if (!Array.isArray(args) && !isArrayLike(args)) {  
            throw new TypeError('myApply 第二个参数不为数组并且不为类数组对象抛出错误');  
        } else {  
            args = Array.from(args) // 转为数组  
            result = context[specialPrototype](...args); // 执行函数并展开数组，传递函数参数  
        }  
    } else {  
        result = context[specialPrototype](); // 执行函数   
    }  
    delete context[specialPrototype]; // 删除上下文对象的属性  
    return result; // 返回函数执行结果  
};  
  
```

#### 7.2 bind

*   拷贝源函数:
    

*   通过变量储存源函数
    
*   使用Object.create复制源函数的prototype给fToBind
    

*   返回拷贝的函数
    
*   调用拷贝的函数：
    

*   new调用判断：通过instanceof判断函数是否通过new调用，来决定绑定的context
    
*   绑定this+传递参数
    
*   返回源函数的执行结果
    

```
Function.prototype._bind = function (objThis, ...params) {  
    const thisFn = this; // 存储源函数以及上方的params(函数参数)  
    // 对返回的函数 secondParams 二次传参  
    let fToBind = function (...secondParams) {  
        const isNew = this instanceof fToBind // this是否是fToBind的实例 也就是返回的fToBind是否通过new调用  
        const context = isNew ? this : Object(objThis) // new调用就绑定到this上,否则就绑定到传入的objThis上  
        return thisFn.call(context, ...params, ...secondParams); // 用call调用源函数绑定this的指向并传递参数,返回执行结果  
    };  
    if (thisFn.prototype) {  
        // 复制源函数的prototype给fToBind 一些情况下函数没有prototype，比如箭头函数  
        fToBind.prototype = Object.create(thisFn.prototype);  
    }  
    return fToBind; // 返回拷贝的函数  
};  
  
```

#### 7.3 call

*   根据call的规则设置上下文对象,也就是this的指向。
    
*   通过设置context的属性,将函数的this指向隐式绑定到context上
    
*   通过隐式绑定执行函数并传递参数。
    
*   删除临时属性，返回函数执行结果
    

```
Function.prototype._call = function (context, ...arr) {  
    if (context === null || context === undefined) {  
       // 指定为 null 和 undefined 的 this 值会自动指向全局对象(浏览器中为window)  
        context = window   
    } else {  
        context = Object(context) // 值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的实例对象  
    }  
    const specialPrototype = Symbol('特殊属性Symbol') // 用于临时储存函数  
    context[specialPrototype] = this; // 函数的this指向隐式绑定到context上  
    let result = context[specialPrototype](...arr); // 通过隐式绑定执行函数并传递参数  
    delete context[specialPrototype]; // 删除上下文对象的属性  
    return result; // 返回函数执行结果  
};  
  
```

## 8 手写继承
------

#### 8.1 构造函数式继承

构造函数式继承并没有继承父类原型上的方法。

```
function fatherUser(username, password) {  
  let _password = password   
  this.username = username   
  fatherUser.prototype.login = function () {  
      console.log(this.username + '要登录父亲账号，密码是' + _password)  
  }  
}  
  
function sonUser(username, password) {  
  fatherUser.call(this, username, password)  
  this.articles = 3 // 文章数量  
}  
  
const yichuanUser = new sonUser('yichuan', 'xxx')  
console.log(yichuanUser.username) // yichuan  
console.log(yichuanUser.username) // xxx  
console.log(yichuanUser.login()) // TypeError: yichuanUser.login is not a function  
```

#### 8.2 组合式继承

```
function fatherUser(username, password) {  
  let _password = password   
  this.username = username   
  fatherUser.prototype.login = function () {  
      console.log(this.username + '要登录fatherUser，密码是' + _password)  
  }  
}  
  
function sonUser(username, password) {  
  fatherUser.call(this, username, password) // 第二次执行 fatherUser 的构造函数  
  this.articles = 3 // 文章数量  
}  
  
sonUser.prototype = new fatherUser(); // 第二次执行 fatherUser 的构造函数  
const yichuanUser = new sonUser('yichuan', 'xxx')  
```

#### 8.3 寄生组合继承

上面的继承方式有所缺陷，所以写这种方式即可。

```
function Parent() {  
  this.name = 'parent';  
}  
function Child() {  
  Parent.call(this);  
  this.type = 'children';  
}  
Child.prototype = Object.create(Parent.prototype);  
Child.prototype.constructor = Child;  
```
