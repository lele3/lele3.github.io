---

title: JavaScript 设计模式之代理模式
key: 20190612
tags: javascript

---

<center><h1>JavaScript设计模式之代理模式</h1></center>

#### 介绍
- 使用者无权访问目标对象
- 中间加代理， 通过代理做授权和控制
#### 举例
代理模式， 用户无权直接访问， 需要找一个代理人间接进行访问， 例如公司内网，当你在家提交代码的时候，你必须使用VPN做为代理，访问公司内网， 完成相关工作
还有就是下面代码示例， 明星经纪人， 外界访问明星，必须经过经纪人，例如你要邀请明星做演出，你不会直接和明星谈论价格，需要通过经纪人来完成此项操作。

#### UML类图
![代理模式UML类图](https://raw.githubusercontent.com/lele3/markDownImages/master/images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8FUML%E7%B1%BB%E5%9B%BE.jpg)
#### 代码
```javascript
class ReadImg {
  constructor(filename) {
    this.filename = filename;
    this.loadFromDisk();
  }
  display () {
    console.log('display...' + this.filename);
  }
  loadFromDisk () {
    console.log('loading..' + this.filename);
  }
}
class ProxyImg {
  constructor (filename) {
    this.realImg = new ReadImg(filename);
  }
  // 接口必须与被代理的对象一致
  display () {
    this.realImg.display();
  }
}
let proxyImg = new ProxyImg('a.png');
proxyImg.display();
```
#### 使用场景
- 网页事件代理

- jQuery $.proxy
```javascript
$('#div').click(function () {
  setTimeout($.proxy(function() {
    $(this).addClass('black');
  }, this), 1000)
})
```

- ES6 Proxy

```javascript
let star = {   // 明星代理人
  name: 'XXX',
  age: 24,
  phone: '13735824459'
}

let agent = new Proxy(star, {
  get: function (target, key) {
    if (key === 'phone') {
      // 经纪人自己手机号
      return '18356342176';
    }
    if (key === 'price') {
      // 明星不报价， 经纪人报价
      return 12000;
    }
    return target[key];
  },
  set: function (target, key, val) {
    if (key === 'customPrice') {
      if (val < 10000) {
        throw new Error('价格低');
      } else {
        target[key] = val;
        return true; // 返回true, 否则赋值失败
      }
    }
  }
})
console.log(agent.phone);
console.log(agent.age);
console.log(agent.price);

agent.customPrice = 15000;
console.log(agent.customPrice);
```
