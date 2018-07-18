下面来进行最后一个模块的介绍，函数式组件functional，这个东西的用法就见仁见智了，这里也没啥好的方案，只是给出了一些示例，各位大佬如果有一些具体的使用到的地方，阔以指点一下哇~thx~\(害羞.jpg\)。

_官方文档的定义是functional组件需要的一切都是通过上下文传递，包括：_

* _props：提供所有 prop 的对象_
* _children: VNode 子节点的数组_
* _slots: 返回所有插槽的对象的函数_
* _data：传递给组件的数据对象，作为 createElement 的第二个参数传入组件_
* _parent：对父组件的引用_
* _listeners: \(2.3.0+\) 一个包含了所有在父组件上注册的事件侦听器的对象。这只是一个指向 data.on 的别名。_
* _injections: \(2.3.0+\) 如果使用了 inject 选项，则该对象包含了应当被注入的属性。_

_在添加 `functional: true`之后，组件的render函数会增加第二个参数context（第一个是createElement），数据和节点通过context传递。_

_**Tips：**_

_在 2.3.0 之前的版本中，如果一个函数式组件想要接受 props，则`props`选项是必须的。在 2.3.0 或以上的版本中，你可以省略`props`选项，所有组件上的属性都会被自动解析为 props。_

---

**我个人的理解是：**

Functional相当于一个纯函数一样，内部不存储用于在界面上展示的数据，传入什么，展示什么，传入的是相同的数据，展示的必然是相同的。无实例，无状态，没有this上下文，均通过context来控制。

**优点：**

因为函数式组件只是一个函数，所以渲染开销低很多。

**使用场景：**

1. 仅仅作为接收参数用的组件（不做任何管理和监听状态）
2. 目前看大多数使用场景是用来包装动画组件，因为动画组件不需要状态管理这些
3. _程序化地在多个组件中选择一个（官方）_
4. _在将 children, props, data 传递给子组件之前操作它们（官方）_

接下来就通过两个组件来看看如何使用的吧，这里也仅仅只是示例而已，使用的场景仍在探索中，具体的使用场景还需要在开发过程中根据需求复杂的和性能要求来酌情选择~

#### 函数式组件一 `wii-functional`用在动画的functional

这个Demo的作用是在输入框中输入字符，对数据列表进行筛选，筛选时加入显示和消失的动画。

**组件主体**

```js
<script>
import Velocity from 'velocity-animate' // 这是一个动画库
export default {
  name: 'wii-functional',
  functional: true, //表明是函数式组件
  render: function(createElement, context) {
    // context是在functional: true时的参数
    let data = {
      props: {
        tag: 'ul',
        css: false
      },
      on: {
        // 进入前事件
        beforeEnter: function(el) {
          el.style.opacity = 0
          el.style.height = 0
        },
        // 进入事件
        enter: function(el, done) {
          let delay = el.dataset.index * 150
          setTimeout(function() {
            Velocity(el, {
              opacity: 1,
              height: '1.6em'
            }, {
              complete: done
            })
          }, delay)
        },
        // 离开事件
        leave: function(el, done) {
          let delay = el.dataset.index * 150
          setTimeout(function() {
            Velocity(el, {
              opacity: 0,
              height: 0
            }, {
              complete: done
            })
          }, delay)
        }
      }
    }
    return createElement('transition-group', data, context.children)
  }
}
</script>
```

上面这个组件相当于创建了一个`ul-li`标签组成的vue动画，通过functional方式包裹到组件外部，可以作为通用的动画。

**引入方式**

```js
<template>
  <div id="app">
    <input v-model="query"/>
    <wii-functional>
      <li v-for="(item, index) in computedList"
          :key="item.msg"
          :data-index="index">
          {{item.msg}}
      </li>
    </wii-functional>
  </div>
</template>

<script>
import WiiFunctional from './components/functional/index.vue'

export default {
  name: 'app',
  components: {
    WiiFunctional
  },
  data() {
    return {
      // 关键字
      query: '',
      // 数据列表
      list: [{
        msg: 'Bruce Lee'
      }, {
        msg: 'Jackie Chan'
      }, {
        msg: 'Chuck Norris'
      }, {
        msg: 'Jet Li'
      }, {
        msg: 'Kung Furry'
      }, {
        msg: 'Chain Zhang'
      }, {
        msg: 'Iris Zhao'
      }, ]
    }
  },
  computed:{
    computedList: function() {
      var vm = this
      // 过滤出符合条件的查询结果
      return this.list.filter(function(item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  watch: {
    computedList(newVal, oldVal) {
      console.log(newVal)
    }
  }
}
</script>
```

#### 函数式组件二 `wii-choose-comp`用在组件切换的functional

在这个示例中，通过props来切换加载不同的组件，并且在props传递给子组件之前操作它，组件内部定义了不同的click事件来展示示例。如果对一批组件进行同样的操作，则可以用这个functional，类似于加工厂。

当然如果组件需要不同的点击事件或者表现方式也可以在各个组件内部单独写逻辑或者监听~因为`wii-choose-comp`这个外壳本质不过就是个函数而已~





