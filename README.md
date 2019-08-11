# front-end
front-end learning

## vue组件的知识
组件的使用包括了定义、注册、使用等过程。
一 组件定义并注册的方法有：
1. 在js文件中：
（1）定义：
var MyComponent = {{
  data: function () {
    return {
      // 数据
    }
  },
  template: '<div>组件内容</div>'
}
（2）注册
//全局注册
Vue.omponent('my-component', MyComponent)
//局部注册，在注册实例的作用域下有效
new Vue({
    components: {
        'my-component': MyComponent
    }
})

2. 单组件文件：在：*.vue文件中(vue文件要包含<template>、<script>、<style>)：
（1）定义
<template>
    <div>组件内容</div>
</template>

<script>
  export default {
      data () {
        return {
          // 数据
        }
      }
  }
</script>

<style scoped>
    div{
        color: red
    }
</style>
（2）注册
import MyComponent from './MyComponent.vue'

export default {
  components: {
    MyComponent // ES6 语法，相当于 MyComponent: MyComponent
  }
}
（还有自动注册以后再说）

二 使用组件
已经注册的组件即可作为自定义的标签在html中使用

三 组件的传值
父与子组件的传值是单向的，为了防止子组件意外修改父组件的值，子组件向父组件传值需要进行另外的处理。

1. 父组件向子组件传值：使用props。
子组件定义：
// ChildComponent.vue
<template>
    <div>
      <b>子组件：</b>{{message}}
    </div>
</template>

<script>
  export default {
    name: "ChildComponent",
    props: ['message']
  }
</script>
父组件定义：
父组件可直接传单个数据值，也可以可以使用指令v-bind动态绑定数据：// parentComponent.vue
<template>
    <div>
      <h1>父组件</h1>
      <ChildComponent message="父组件向子组件传递的非动态值"></ChildComponent>
      <input type="text" v-model="parentMassage"/>
      <ChildComponent :message="parentMassage"></ChildComponent>
    </div>
</template>

<script>
  import ChildComponent from '@/components/ChildComponent'
  export default {
    components: {
      ChildComponent
    },
    data () {
      return {
        parentMassage: ''
      }
    }
  }
</script>

2. 子组件向父组件传值：子组件中使用 $emit()触发自定义事件，父组件使用$on()监听
子组件：
// ChildComponent.vue
<template>
  <div>
    <b>子组件：</b><button @click="handleIncrease">传递数值给父组件</button>
  </div>
</template>

<script>
  export default {
    name: "ChildComponent",
    methods: {
      handleIncrease () {
        this.$emit('increase',5)
      }
    }
  }
</script>

父组件：
// parentComponent.vue
<template>
    <div>
      <h1>父组件</h1>
      <p>数值：{{total}}</p>
      <ChildComponent @increase="getTotal"></ChildComponent>
    </div>
</template>

<script>
  import ChildComponent from '@/components/ChildComponent'
  export default {
    components: {
      ChildComponent
    },
    data () {
      return {
        total: 0
      }
    },
    methods: {
      getTotal (count) {
        this.total = count
      }
    }
  }
</script>
父组件可以get到子组件的increase值。
