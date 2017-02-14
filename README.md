# 关于 Vue 组件，你需要知道的一些事

<details>
You Need Something About Vue Component.
</details>

![Vue Component](./component.png)

Vue.js 是一套构建用户界面的渐进式框架。我们可以使用简单的 API 来实现响应式的数据绑定和组合的视图组件。

从维护视图到维护数据，Vue.js 让我们快速地开发应用。但随着业务代码日益庞大，组件也越来越多，组件逻辑耦合严重，使代码维护变得十分困难。

同时，Vue.js 的接口和语法十分自由，实现同一功能有若干种方法。每个人解决问题的思路不一样，写出来的代码也就不一样，缺乏团队内的规范。

本文旨在从组件开发的不同方面列举出合理的解决方法，作为建立组件规范的一个参考。

## 导航

1. [构成组件](#part)
2. [组件状态传递](#communication)
3. [业务组件与可复用组件](#businessAndReusable)
4. [自由组合](#freeCombination)
5. [结构扁平化](#flattingConstruction)
6. [数据扁平化](#flattingData)
7. [双向绑定](#bidirectionalBindings)
8. [项目骨架](#skeleton)
9. [其他细节](#other)

## <a id="part">构成组件</a>

组件，是一个具有一定功能，且不同组件间功能相对独立的模块。组件可以是一个按钮、一个输入框、一个视频播放器等等。

可复用组件，高内聚、低耦合。

## <a id="communication">组件状态传递</a>
