---

title: JavaScript 设计模式之适配器模式
key: 20190603
tags: javascript

---

<center><h1>JavaScript设计模式之适配器模式</h1></center>


#### 介绍
- 旧接口与使用者不兼容
- 中间加一个适配转换接口

#### 举例

使用mac的人都知道，mac的usb接口和目前市场上流行的接口不太一样，需要使用一个转换器来连接， 这个转换器就相当于程序中的适配器。

#### UML类图
![适配器模式的UML类图](https://raw.githubusercontent.com/lele3/markDownImages/master/images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8FUML%E7%B1%BB%E5%9B%BE.jpg)
#### 代码

```javascript
class Adaptee {
  specificRequest() {
    return 'mac版usb接口'
  }
}

class Target {
  constructor () {
    this.adaptee = new Adaptee()
  }
  request () {
    let info = this.adaptee.specificRequest()
    return `${info} - 转换器 - 市场普通usb`
  }
}

let target = new Target()
target.request()
```

#### 使用场景

1. 旧接口转换为新接口
   假如以前代码中的ajax请求全部都是用jquery的ajax写的，但是目前流行的比如axios等库， 就可以自己将axios封装成ajax形式的
   ```javascript
   // 自己封装的ajax，使用方式如下
   ajax({
     url: 'xxx',
     type: 'POST',
     dataType: 'json',
     data: {
       id: 123
     }
   })
   .done((res) => {})

   // 历史遗留， 代码中全是
   // $.ajax({...})
   ```
   如果自己一个一个替换，显然是不合适的。所以做一层封装转换
   ```javascript
   var $ = {
     ajax: function (options) {
       return ajax(options)
     }
   }
   ```
   这样程序中已有的代码不需要修改，旧代码也可以兼容新的ajax请求

2. vue computed
  这个就是一个典型的适配器模式，当data里的数据不满足你渲染的需求时，这时就可以应用计算属性，将其转换成你将要渲染的数据。
  代码省略。