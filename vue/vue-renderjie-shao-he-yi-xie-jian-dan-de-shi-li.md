# Vue Render介绍和一些简单的实例

_Vue 推荐在绝大多数情况下使用 template 来创建你的 HTML。然而在一些场景中，你真的需要 JavaScript 的完全编程的能力，这就是**render 函数**，它比 template 更接近编译器。_（**从官网复制的，慌得一批，其实简单来说就是以函数的方式写HTML，可控性更强一些~**）

当然，官网已经给出了一个使用template来编写的不方便的demo，所以在这里就不反复提起了，初次使用或者有兴趣的大佬可以直接戳这个链接了解一下~[Vue Render](https://cn.vuejs.org/v2/guide/render-function.html#基础 "Vue Render")

### 本篇文章主要介绍了以下几点

> 了解基本概念的客官可以直接下拉到实例，实例已上传github，传送门[vue-render-test](https://github.com/godaangel/vue-render-test.git)

* 虚拟DOM
* CreateElement函数
* 【起步】最基本的实例
* 【进阶】包含属性配置较完整的实例
* 【进阶】CreateElement中`slot`属性的用法
* 【深入】CreateElement中`scopedSlots`的用法
* 【换口气儿】Render中的JSX配置和用法
* 【深入】函数式组件

### 虚拟DOM

##### 1、**什么是DOM？**

DOM 就是浏览器解析 HTML 得来的一个树形逻辑对象。

##### 2、**什么是虚拟DOM？**

用 Object 来代表一个节点，这个节点叫做虚拟节点（ Virtual Node ）简写为 VNode，由 VNode 树组成虚拟DOM。

##### 3、**为什么要用虚拟DOM？**

Web 页面的大多数操作和逻辑的本质就是不停地修改 DOM 元素，但是 DOM 操作太慢了，过于频繁的 DOM 操作可能会导致整个页面掉帧、卡顿甚至失去响应。仔细想一想，很多 DOM 操作是可以打包（多个操作压成一个）和合并（一个连续更新操作只保留最终结果）的，同时 JS 引擎的计算速度要快得多，所以为什么不把 DOM 操作先通过JS计算完成后统一来一次大招操作DOM呢，于是就有了虚拟DOM的概念。当然，虚拟DOM操作的核心是Diff算法，也就是比较变化前后Vnode的不同，计算出最小的DOM操作来改变DOM，提高性能。

##### 4、**Vue里面的虚拟DOM怎么生成的呢？**

通过\`createElement\(tag, options, VNodes\)\`，下面就来介绍这个函数的基本概念。

### CreateElement 函数

> 简单来说CreateElement就是用来生成Vnode的函数

CreateElement 到底会返回什么呢？其实不是一个实际的 DOM 元素（返回的是Vnode）。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，及其子节点。

**【Tips】**`CreateElement`**函数在惯例中通常也写作**`h`

##### 1、**CreateElement的参数如下所示**：（~~太懒了直接搬的官网~~）

```js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签字符串，组件选项对象，或者 解析上述任何一种的一个 async 异步函数，必要参数。
  'div',

  // {Object}
  // 一个包含模板相关属性的数据对象
  // 这样，您可以在 template 中使用这些属性。可选参数。
  {
    // 详情见下方
  },

  // {String | Array}
  // 子节点 (VNodes)，由 `createElement()` 构建而成，或使用字符串来生成“文本节点”。可选参数。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

【**Tips】: **文档中此处说VNodes子节点必须是唯一的，也就是说第三个参数的Array里不能出现相同指向的VNodes，实际验证以后，就算写重复的VNodes，也并不会报错，估计此处会有些坑，现在还没踩到，建议按照文档要求，保持子节点唯一。

##### 2、Vnode属性配置（第二个参数）：\(~~太懒了也是直接搬，捂脸.jpg~~\)

以下属性为简单介绍，具体用法和一些 _备注解释 _可以参考后面会讲到的【包含属性配置较完整的实例】

```js
{
  // 和`v-bind:class`一样的 API
  // 接收一个字符串、对象或字符串和对象组成的数组
  'class': {
    foo: true,
    bar: false
  },
  // 和`v-bind:style`一样的 API
  // 接收一个字符串、对象或对象组成的数组
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 正常的 HTML 特性
  attrs: {
    id: 'foo'
  },
  // 组件 props
  props: {
    myProp: 'bar'
  },
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器基于 `on`
  // 所以不再支持如 `v-on:keyup.enter` 修饰器
  // 需要手动匹配 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅对于组件，用于监听原生事件，而不是组件内部使用
  // `vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
  // 赋值，因为 Vue 已经自动为你进行了同步。
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // 作用域插槽格式
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 如果组件是其他组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
  // 其他特殊顶层属性
  key: 'myKey',
  ref: 'myRef'
}
```

### 【起步】最基本的实例

这是一个基础的Demo，包含了

* 简单的渲染用法

* 标签

* props

* slot

* 点击事件

以下示例Demo均采用单文件组件的方式，工程用`vue-cli`搭建的`webpack-simple`工程。

**组件**`wii-first`

```js
export default {
  name: 'wii-first',
  data() {
    return {
      msg: 0
    }
  },
  props: {
    level: {
      type: [Number, String],
      required: true
    }
  },
  render: function(createElement) {
    this.$slots.subtitle = this.$slots.subtitle || []
      // this.level = 1时, 等价于
      // <h1 class="wii-first">
      //  第一个组件, <slot></slot>
      //  <slot name="subtitle"></slot>，此处是data的值: {{msg}}
      //  <button @click="clickHandler">点我改变内部data值</button>
      // </h1>
    return createElement(
      'h' + this.level, // tag name 标签名称
      {
        class: 'wii-first'
      },
      // this.$slots.default, // 子组件中的slot 单个传递
      // this.$slots.subtitle,
      [
        '第一个组件, ',
        ...this.$slots.default, // 具名slots传递 此处也可以用children来代替
        ...this.$slots.subtitle,
        '，此处是data的值: ',
        this.msg,
        createElement('button', {
          on: {
            click: this.clickHandler
          },
        }, '点我改变内部data值')
      ]
    )
  },
  methods: {
    clickHandler() {
      this.msg = Math.ceil(Math.random() * 1000)
    }
  }
}
```

**引入方式**

```js
<template>
  <div id="app">
    <wii-first level="1">我是标题 <span slot="subtitle">我是subtitle</span></wii-first>
  </div>
</template>

<script>
import WiiFirst from './components/first/index.vue'
export default {
  name: 'app',
  components: {
    WiiFirst
  },
  data() {
    return {

    }
  }
}
</script>
```

### 【进阶】包含属性配置较完整的实例

这个Demo主要展示了createElement属性用法，包含

* class，style，attrs，on等
* 包含了点击事件以及`click.stop`的转换示例
* 事件 & 按键修饰符

**组件`wii-second`**

```

```





