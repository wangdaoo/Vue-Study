/*
 * @Author: MARS 
 * @Date: 2019-03-18 21:43:39 
 * @Last Modified by: mikey.zhaopeng
 * @Last Modified time: 2019-03-21 13:55:43
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