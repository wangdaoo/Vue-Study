/*
 * @Author: MARS 
 * @Date: 2019-03-18 21:43:39 
 * @Last Modified by: mikey.zhaopeng
 * @Last Modified time: 2019-03-21 20:03:27
 */
# Vue 开发实践

## 特点

* 轻量
* 渐进式框架
* 响应式的更新机制
* 学习成本低

## 内容概述

* **基础：** Vue 核心知识点
* **生态：** 大型 Vue 项目所需的周边技术
* **实战：** 开发基于 Vue 的 Ant Design Pro
* **福利：** Vue 3.0 相关知识介绍

## 环境搭建

* `浏览器`：Chrome
* `IDE`：VS Code
* `Node.js`： v10.15.0
* `npm`： 6.4.1

## `Vue.component` 缺点

* **全局定义：** 强制要求每个 `component` 中的命名不得重复
* **字符串模板：** 缺乏语法高亮，在`HTML`有多行的时候，需要用到丑陋的` \ `
* **不支持 css：** 意味着当 `HTML` 和 `JavaScript` 组件化时，`CSS` 明显被遗漏
* **没有构建步骤：** 限制只能使用 `HTML` 和 `ES5 JavaScript`，而不能使用预处理器，如 `Pug(formerly Jade)` 和 `Bable`

## Vue CLI

* npm install -g @vue/cli
* vue create Vue-Study
* cd Vue-Study
* npm run serve

## 组件的概念

小型的、独立的、可以复用的`UI`模块

### 三大核心概念

* 属性
  * 自定义属性 `props`, 组件`props`中声明的属性
  * 原生属性 `attrs`, 没有声明的属性，默认自动挂载到组件根元素上，设置 `inheritAttrs` 为 `false` 可以关闭自动挂载
  * 特殊属性 `class`、`style`, 挂载到组件根元素上，支持字符串、对象、数组等多种语法
* 事件
  * 普通事件 `@click`、`@input`、`@change`等事件，通过`this.$emit('xxx', ...)` 触发
  * 修饰符事件 `@input.trim`、`@click.stop`、`@submit.prevent`等，一般用于原生`HTML`元素，自定义组件需要自行开发支持
* 插槽

## 生命周期

生命周期函数就是 vue 实例在某一个时间点会自动执行的函数

```html
  <div id="app"></div>
  <script>
    let vm = new Vue({
      el: '#app',
      template: `<h1>{{ msg }}</h1>`,
      data: {
        msg: 'Hello World!'
      },
      /**
       * 检测是否有 el 属性
       * 检测是否有 template 属性，
       * 如果有 template 模板属性，就会用模板去渲染；
       * 如果没有 template 模板，就会把 el 外层的 HTML 当做模板去渲染
       */
      beforeCreate() {
        console.log('beforeCreate')
        // 在 new Vue() 实例之前会执行beforeCreate方法
      },
      created() {
        console.log('created')
      },
      beforeMount() {
        console.log(this.$el)
        // <div id="app"></div>
        // 在 beforeMount 的时候，数据并没有被渲染到页面上
        console.log('beforeMount')
      },
      mounted() {
        console.log(this.$el)
        // <h1>Hello World!</div>
        // mounted 的时候，数据已经渲染到页面之上
        console.log('mounted')
      },
      beforeUpdate() {
        console.log('beforeUpdate')
        // 当数据发生改变是执行
      },
      updated() {
        console.log('updated')
        // 数据改变完成之后执行
      },
      beforeDestroy() {
        console.log('beforedestroy')
        // 数据销毁前执行
      },
      destroyed() {
        console.log('destroyed')
        // 数据销毁完毕，over
      },
    })
  </script>
```

**`Vue` 的生命周期函数，并不放在 `methods`里面，而是放在`Vue`实例里**

## `Vue` 模板语法

* `{{ message }}` 插值表达式 = `v-text`
*  `v-html` 会将`HTML`模板编译为DOM

## 计算属性，方法，侦听器

* **`computed`**

```html
<div id="app">
    <h1>{{ fullname }}</h1>
    <h2>{{ age }}</h2>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            firstname: 'Wong',
            lastname: 'MARS',
            age: 23
        },
        // 计算属性
        // 内部存在缓存机制
        // 改变属性不会重新计算
        // 性能较高
        computed: {
            fullname: function () {
                console.log('计算了一次')
                return this.firstname + ' ' + this.lastname
            }
        }
    })
</script>
```

* **`methods`**

```html
<div id="app">
    <h1>{{ fullname() }}</h1>
    <!-- 
      {{ fullname() }}  
      调用fullname 方法
    -->
    <h2>{{ age }}</h2>
</div>
<script>
  let vm = new Vue({
      el: '#app',
      data: {
          firstname: 'Wong',
          lastname: 'MARS',
          age: 23
      },
      // 方法
      // 方法，内部没有缓存机制
      // 如果改变了一次属性，会重新计算一次
      // 性能较低
      methods: {
          fullname: function () {
              console.log('计算了一次')
              return this.firstname + ' ' + this.lastname
          }
      }
  })
</script>
```

* **`watch`**

```html
<div id="app">
    <h1>{{ fullname }}</h1>
    <h2>{{ age }}</h2>
</div>
<script>
  let vm = new Vue({
      el: '#app',
      data: {
          firstname: 'Wong',
          lastname: 'MARS',
          fullname: "Wong MARS",
          age: 23
      },
      // 相对复杂
      watch: {
          firstname: function () {
              console.log('侦听到一次 firstname')
              this.fullname = this.firstname + ' ' + this.lastname
          },
          lastname: function () {
              console.log('侦听到一次 lastname')
              this.fullname = this.firstname + ' ' + this.lastname
          }
      }
  })
</script>
```

## `getter , setter`

```html
<div id="app">
    {{ fullname }}
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            firstname: 'Wong',
            lastname: 'MARS'
        },
        computed: {
            fullname: {
                get: function () {
                    // 读取属性
                    return this.firstname + ' ' + this.lastname
                },
                set: function (value) {
                    // 设值属性
                    console.log(value)
                    var arr = value.split(" ")
                    this.firstname = arr[0]
                    this.lastname = arr[1]
                }
            }
        },
    })
</script>
```

## 样式绑定

样式绑定都有两种方式进行

- 对象
- 数组

```html
<!-- 通过 class 进行绑定样式 -->

<style>
    .actived {
        color: red;
    }
</style>

<div id="app">
    <div @click="handleClick" :class="{actived : isactiveed}">Hello World</div>
</div>

<script>
    let vm = new Vue({
        el: '#app',
        data: {
            isactiveed: false
        },
        methods: {
            handleClick: function () {
                this.isactiveed = !this.isactiveed
                // 取反
            }
        },
    })
</script>
```

```html
<!-- 通过 style 方式进行样式绑定 -->

<div id="app">
    <div :style="[styleObj]" @click="handleClick">Hello World</div>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            styleObj: {
                color: 'black',
                fontSize: '20px'
            }
        },
        methods: {
            handleClick: function () {
                this.styleObj.color = this.styleObj.color === 'black' ? 'red' : 'black'

                this.styleObj.fontSize = this.styleObj.fontSize === '20px' ? '30px' : '20px'
            }
        },
    })
</script>
```

## 条件渲染

```html
<!-- 
    给每个 input 加 key 值，不会复用其中的内容
 -->
<div id="app">
    <div v-if="show">
        用户：<input v-model="msg" key="username" type="text">
        <h1>Message is:{{ msg }}</h1>
    </div>
    <div v-else>
        邮箱：<input v-model="msg1" key="pwd" type="text">
        <h1>Message is:{{ msg1 }}</h1>
    </div>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg: '',
            msg1: '',
            show: false
        }
    })
</script>
```

## 父子组件传值

```html
<!-- 
    父组件通过属性的形式向子组件进行传值
    
    子组件通过事件触发向子组件传值

    父组件可以向子组件传递任何数据，但是子组件不能修改父组件传递过来的数据

    如果必须要修改父组件数据，就需要copy一个副本出来
 -->

<div id="app">
    <!-- 子组件向父组件传值 -->
    <counter :count='0' @inc="handleIncrease"></counter>
    <counter :count='1' @inc="handleIncrease"></counter>

    <!-- 父组件向子组件传值 -->
    <h1>{{ total }}</h1>
</div>
<script>
    let counter = {
        props: ['count'],

        // -----------------------
        // 子组件修改父组件传递的数据，copy 一个副本出来
        data: function () {
            return {
                number: this.count
            }
        },
        // -----------------------
        
        template: `<h1 @click="handleClick">
            {{ number }}
        </h1>`,
        methods: {
            handleClick: function () {
                this.number = this.number + 2
                this.$emit('inc', 2)
            }
        },
    }

    let vm = new Vue({
        el: '#app',
        data: {
            total: 1
        },
        components: {
            counter: counter
        },
        methods: {
            handleIncrease: function (step) {
                console.log('inc')
                this.total += step
            }
        }
    })
</script>
```

## 插槽 `slot`

```html
<!-- 
    slot 
        在模板内插入 slot 
        如果有很多 slot ，就需要给 slot 一个 name 属性，避免 slot 被重复使用

        这种语法叫做具名插槽，给每个 slot 起一个名字
 -->
<div id="app">
    <child>
        <h2 class="header" slot="header">Header</h2>
        <h2 class="footer" slot="footer">Footer</h2>
    </child>
</div>
<script>
    Vue.component('child', {
        template: `
            <div>
                <slot name="header">
                    <code>default Header</code>
                </slot>
                <h1 class="content">Content</h1>
                <slot name="footer"></slot>
            </div>
        `
        // <code>default Header</code>
        // 默认值  如果上面 DOM 结构中没有引入 slot 就会显示 defalut Header 默认值
    })
    let vm = new Vue({
        el: '#app'
    })
</script>
```

## css 动画过渡

### 过渡的类名

* `v-enter` : 定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除
* `v-enter-active` : 定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入的过渡的过程时间，延迟和曲线函数
* `v-enter-to` : 定义进入过渡的结束状态，在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。
* `v-leave` : 定义离开过渡的开始状态.在离开过渡被触发时立刻生效，下一帧被移除
* `v-leave-active` : 定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡时立刻生效，在过渡/动画完成之后移除。这个类可以用来定义离开过渡的过程时间，延迟和曲线函数
* `v-leave-to` : 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效（与此同时 `v-leave` 被删除），在过渡/动画完成之后移除