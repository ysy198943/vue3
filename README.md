## vue3
### vue3的优化
#### 结构上的优化 => monorepo
原子结构 可独立拆分引用 => 可做业务上的拆分
#### 性能上的优化
##### 移除了很多使用率低的api
##### 产物打包优化 => tree-shaking
##### 编译优化
compile阶段静态模板进行分析 => 分析树 <= PatchFlag
##### 数据劫持优化
Object.defineProperty
1. 无法检测对象属性的增加或删除 => $set $delete
2. 数组
3. 层级较深的对象 => 递归遍历
Proxy => 底层优化

### 模板编译
1. 词法分析阶段(baseParse)：template => AST (节点类别的区分)
2. 指令和语法的转化阶段（transform）：AST => 解析不同的节点进行区分 => 不同类型的转换
3. 可执行函数的生成阶段(generate): 转化后的AST生成渲染函数

* 主编译入口：core/packages/compiler-core/src/index.ts

### 基于Proxy的响应式 === defineReactive
1. 数据劫持 | 数据响应（reactive）: 数据变化 => 函数监听执行
2. 依赖收集（effect函数 | 副作用函数）
   当前vm实例上挂载effect => 当前activeEffect切换为effect => 在effect上创建deps等属性，用于传递依赖
2.5 
   2.5.1 结合1 + 2，变量访问 => 触发对应的get() => 创建deps对象（targetMap） => targetMap中的deps可以作为属性进行添加 (depsMap)  ---- 对应vue2的defineReactive
   2.5.2 depsMap会被添加activeEffect - 被收集的订阅方
         activeEffect中也同时存在deps数组用于存放关联方的depsMap - 订阅者
3. 派发更新（ref）
   依赖的set()被触发 => Reflect.set()修改对应的属性 => 获取到targetMap订阅方（depsMap） => 链条传递 => 触发渲染

* 主响应式入口：/core/packages/reactivity/src/index.ts


# vuejs/core [![npm](https://img.shields.io/npm/v/vue.svg)](https://www.npmjs.com/package/vue) [![build status](https://github.com/vuejs/core/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/vuejs/core/actions/workflows/ci.yml)

## Getting Started

Please follow the documentation at [vuejs.org](https://vuejs.org/)!

## Sponsors

Vue.js is an MIT-licensed open source project with its ongoing development made possible entirely by the support of these awesome [backers](https://github.com/vuejs/core/blob/main/BACKERS.md). If you'd like to join them, please consider [ sponsoring Vue's development](https://vuejs.org/sponsor/).

<p align="center">
  <h3 align="center">Special Sponsor</h3>
</p>

<p align="center">
  <a target="_blank" href="https://github.com/appwrite/appwrite">
  <img alt="special sponsor appwrite" src="https://sponsors.vuejs.org/images/appwrite.svg" width="300">
  </a>
</p>

<p align="center">
  <a target="_blank" href="https://vuejs.org/sponsor/#current-sponsors">
    <img alt="sponsors" src="https://sponsors.vuejs.org/sponsors.svg?v3">
  </a>
</p>

## Questions

For questions and support please use [the official forum](https://forum.vuejs.org) or [community chat](https://chat.vuejs.org/). The issue list of this repo is **exclusively** for bug reports and feature requests.

## Issues

Please make sure to respect issue requirements and use [the new issue helper](https://new-issue.vuejs.org/) when opening an issue. Issues not conforming to the guidelines may be closed immediately.

## Stay In Touch

- [Twitter](https://twitter.com/vuejs)
- [Blog](https://blog.vuejs.org/)
- [Job Board](https://vuejobs.com/?ref=vuejs)

## Contribution

Please make sure to read the [Contributing Guide](https://github.com/vuejs/core/blob/main/.github/contributing.md) before making a pull request. If you have a Vue-related project/component/tool, add it with a pull request to [this curated list](https://github.com/vuejs/awesome-vue)!

Thank you to all the people who already contributed to Vue!

<a href="https://github.com/vuejs/core/graphs/contributors"><img src="https://opencollective.com/vuejs/contributors.svg?width=890" /></a>

## License

[MIT](https://opensource.org/licenses/MIT)

Copyright (c) 2013-present, Yuxi (Evan) You
