---
title: html中的script标签
key: 20200213
tags: html
---
<center><h1>html中的script标签</h1></center>



### script标签

对于一名前端开发人员来说，`script`标签应该是接触最早的标签了，但是对于它所拥有的属性，到目前还是缺乏了解，遂记录如下

<!--more-->

#### 功能

向html文档中插入JavaScript脚本

方法一

```html
<script type='text/javascript'>
  document.write('Hello World!')
</script>
```

方法二

```html
<script type='text/javascript' src='hello.js'>
</script>
```

#### 属性

在HTML5的script标签中主要有这几个属性 async，defer，src，type

**async**

可选。表示应该立即下载脚本，但不妨碍页面中的其他操作，只对外部脚本文件有效。下载完成后停止解析并执行该脚本， 执行完成以后继续执行解析。当有多个script脚本被指定为async, 并不能保证按照指定他们的先后顺序执行

**defer**

可选。立即下载，延迟执行，下载时不会妨碍页面的其他操作。表示脚本可以延迟到文档完全被解析和显示之后再执行，只对外部脚本文件有效。当有多个script脚本被指定为defer时，HTML5规范要求按照它们出现的先后顺序执行，但是在现实中，延迟脚本并不一定会按照顺序执行， 也不一定会在DOMContentLoaded事件出发前执行，因此最好只包含一个延迟脚本。HTML5规定嵌入脚本的defer不起作用

**src**

可选。表示包含要执行代码的外部文件。

**type**

可选。表示编写代码使用的脚本语言的内容类型（也成为MIME类型），通常为`text/javascript`，如果不指定，默认为`text/javascript`

#### 标签的位置

按照传统的做法，所以`<script>`元素都应该放在页面的`<head>`标签中，例如

```html
<!DOCTYPE html>
<html>
<head>
  <title>Example HTML Page</title>
  <script type="text/javascript" src='./a.js'></script>
  <script type="text/javascript" src='./b.js'></script>
</head>
<body>
  <!-- 这里放内容 -->
</body>
</html>
```

这么做的目的就是把所有的外部文件引用放在一起，便于管理。但是，在`<head>`元素中包含所有`javascript`脚本就必须意味着等`javascript`文件都下载、解析、执行完成以后，才开始呈现页面的内容，这样，就会增加用户看到白屏的时间，对用户很不友好，所以应该等DOM加载完成以后再去下载解析`javascript`文件，即放到`</body>`标签后面，如下

```html
<!DOCTYPE html>
<html>
<head>
  <title>Example HTML Page</title>
</head>
<body>
  <!-- 这里放内容 -->
</body>
<script type="text/javascript" src='./a.js'></script>
<script type="text/javascript" src='./b.js'></script>
</html>
```

#### `<noscript>`标签

当浏览器不支持`javascript`或者禁用`javascript`时，会显示`<noscript>`中的内容，这个元素可以包含能够出现在文档`<body>`中的任何HTML元素——`<script>`元素除外。

例如

```html
<!DOCTYPE html>
<html>
<head>
  <title>Example HTML Page</title>
  <script type="text/javascript" src='./a.js'></script>
  <script type="text/javascript" src='./b.js'></script>
</head>
<body>
  <!-- 这里放内容 -->
  <noscript>
    <p>本页面需要浏览器支持（启用）JavaScript.</p>
  </noscript>
</body>
</html>
```

