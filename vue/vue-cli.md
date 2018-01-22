# 使用vue-cli构建Vue工程

> Vue提供了很方便的脚手架vue-cli，通过该脚手架可以快速创建vue工程项目。

首先，需要全局安装该脚手架。

```
$ npm install vue-cli -g
```

安装成功以后，进入你想放置项目的文件夹，在命令行执行：

```
$ vue init webpack project_name //project_name为你的项目名
```

在选择的时候，推荐禁用`.eslintrc.js`里面的`rules`或者ESLint选择no，如果想用，那么为了不影响开发，可以先如下配置。

```js
'rules': {
    // 下面的配置产生了大量编辑器报错提示
    // 为代码查看方便，暂时禁用
    'semi': 0,
    'quotes': 0,
    'indent': 0,
    'no-tabs': 0,
    'eol-last': 0,
    'comma-spacing': 0,
    'key-spacing': 0,
    'keyword-spacing': 0,
    'no-trailing-spaces': 0,
    'space-before-blocks': 0,
    'no-mixed-spaces-and-tabs': 0,
    'space-before-function-paren': 0,

    // allow paren-less arrow functions
    'arrow-parens': 0,
    // allow async-await
    'generator-star-spacing': 0,
    // allow debugger during development
    'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0
  }
```

---

##### PS：构建简单工程（组件框架开发用）

执行 `$ vue init webpack project_name //project_name为你的项目名`

在配置文件的output下配置如下代码

`library: 'wy.bupt' //此处可替换`

`libraryTarget: 'umd'`

`umdNamedDefine: true`

即可输出你自己的组件框架。通过`$ npm publish` 即可发布。



