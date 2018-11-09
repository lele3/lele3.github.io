---
title: vue项目中引入jquery
key: 20181108
tags: vue
---

<center><h2>vue项目中引入jquery</h2></center>
<!--more-->
- npm 安装 cnpm install jquery --save

- 找到webpack.base.config.js

  - 在最开始的地方加入如下代码 `const webpack = require('webpack')`

  - 在module.export中加入如下代码

    ```
    plugins: [
    	new webpack.ProvidePlugin({
    		$: 'jquery',jQuery:'jquery'
    	})
    ],
    ```



vue优化

https://mp.weixin.qq.com/s/qaoPSmE29L6fkm2tEH4U0Q