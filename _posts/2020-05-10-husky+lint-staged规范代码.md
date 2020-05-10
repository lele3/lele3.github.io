---
title: husky+lint-staged+prettier规范提交代码
key: 20200510
tags: 前端
---
<center><h1>husky+lint-staged+prettier规范提交代码</h1></center>

当多个人维护同一个项目时，如果没有统一的代码规则，每个人按照每个人的编码习惯去写代码，那对后期代码维护来说可能是灾难性的，即使项目中引入了eslint，有时候也会有人不去遵守，所以这时候就需要一套规范外加自动格式化，当代码提交到远程仓库之前，先按照标准自动格式化一遍， 然后在push到远程仓库，这就完成了代码格式的统一。那么我们就来引出我们今天的主角husky、lint-staged、prettier。
<!--more-->


### husky

在介绍husky之前，我们先来说一说git的hooks函数，hooks函数主要作用就是在特定的重要动作发生时触发的动过，它分为客户端钩子和服务端钩子，我们在此只介绍客户端钩子。

钩子都被存储在 Git 目录下的 `hooks` 子目录中。 也即绝大部分项目中的 `.git/hooks` 。 当你用 `git init` 初始化一个新版本库时，Git 默认会在这个目录中放置一些示例脚本。 这些脚本除了本身可以被调用外，它们还透露了被触发时所传入的参数。 所有的示例都是 shell 脚本，其中一些还混杂了 Perl 代码，不过，任何正确命名的可执行脚本都可以正常使用 —— 你可以用 Ruby 或 Python，或任何你熟悉的语言编写它们。 这些示例的名字都是以 `.sample` 结尾，如果你想启用它们，得先移除这个后缀。（参考自[git文档](https://git-scm.com/book/zh/v2/自定义-Git-Git-钩子))

在此，我们只介绍一个钩子`pre-commit`,该钩子在键入提交信息前运行。 它用于检查即将提交的快照，例如，检查是否有所遗漏，确保测试运行，以及核查代码。 如果该钩子以非零值退出，Git 将放弃此次提交。



现在开始介绍我们的主角`husky`,英语比较棒的同学一样就能看出这不是哈士奇🐶的英文吗 woof!, 没错，你真棒！！！ `husky`是一个npm包，用于帮助我们快速触发`git hooks`函数，而不用去更改`.git/hooks`中的代码，首先安装

```javascript
npm install husky --save-dev
```

然后在`package.json`中添加以下配置

```javascript
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "npm test",
      "...": "..."
    }
  }
}
```

如上当你执行`git commit`的时候，就会自动触发 `npm test`命令



### lint-staged

npm上查找`lint-staged`， 它的介绍中第一句话就是`Run linters against staged git files and don't let 💩 slip into your code base!`, 翻译过来大致意思就是 对暂存的git文件执行linters，不要让💩进入你的代码， 说白了，就是该npm包可以对你git暂存区的代码（也就是你git add的代码）进行扫描，执行你自定义命令

安装

```javascript
npm install lint-staged --save-dev
```

然后在`package.json`中添加以下配置，也可以使用`lint-staged.config.js`、`.lintstagedrc.js`配置文件，更多配置请自行查看文档

```javascript
"lint-staged": {
  "*.{js,jsx}": [
    "prettier --write",   // prettier 自动格式话暂存区代码
    "eslint --cache --ext .js" // eslint 对暂存区代码执行eslint校验， 当添加--cache时，执行命令会生成一个 .eslintcache， 记得把它添加到.gitignore中去
  ]
}
```



### prettier

这个npm包可以自动格式化代码，我们项目中`.prettierrc.js`配置文件如下，**注意： prettier配置不能和eslint的配置冲突**

```javascript
module.exports = {
  // 每行的长度，默认80
  printWidth: 80,

  // 制表符tab的宽度，默认值是2
  tabWidth: 2,

  // 是否使用tab来代替spaces缩进， 默认false
  useTabs: false,

  // 是否需要分号
  semi: true,

  // 是否使用单引号代替双引号
  singleQuote: true,

  // 对象属性的引号使用
  // as-needed 仅在需要的时候使用
  // consistent 有一个属性需要引号，就都需要引号
  // preserve 保留用户输入的情况
  quoteProps: 'preserve',

  // 在jsx中使用单引号替换双引号
  jsxSingleQuote: false,

  // 行尾逗号,默认es5,可选 none|es5|all
  // none 没有行尾逗号
  // es5 包括es5中的数组、对象
  // all 包括函数对象等所有可选
  trailingComma: 'es5',

  // 对象中的空格 默认true
  // true: { foo: bar }
  // false: {foo: bar}
  bracketSpacing: true,

  // JSX标签闭合位置 默认false
  // false: <div
  //          className=""
  //          style={{}}
  //       >
  // true: <div
  //          className=""
  //          style={{}} >
  jsxBracketSameLine: false,

  //箭头函数中的括号
  // “avoid” - 在有需要的时候使用. Example: x => x
  // “always” - 一直使用. Example: (x) => x
  arrowParens: 'avoid',
};

```



所以最终`package.json`中是这样的

```javascript
// package.json
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
},
"lint-staged": {
  "*.{js,jsx}": [
    "prettier --write",
    "eslint --cache --ext .js"
  ]
}
```



配置的比较简单，但是最基本的代码检查、自动格式代码功能已经都有了。