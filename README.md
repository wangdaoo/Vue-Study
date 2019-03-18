/*
 * @Author: MARS 
 * @Date: 2019-03-18 21:43:39 
 * @Last Modified by: MARS
 * @Last Modified time: 2019-03-18 22:39:09
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