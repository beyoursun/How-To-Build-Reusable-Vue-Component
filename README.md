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

那么，什么构成了组件呢。以浏览器的原生组件 video 为例，分析一下组件的组成部分。

```html
<video
  src="example.mp4"
  width="320"
  height="240"
  onload="loadHandler"
  onerror="errorHandler">
  Your browser does not support the video tag.
</video>
```

实例中能看出，组件由*状态*、*事件*和嵌套的*片断*组成。状态，是组件当前的某些数据或属性，如 video 中的 src、width 和 height。事件，是组件在特定时机触发一些操作的行为，如 video 在视频资源加载成果或失败时会触发对应的事件来执行处理。片段，指的是嵌套在组件标签中的内容，该内容会在某些条件下展现出来，如在浏览器不支持 video 标签时显示提示信息。

在 Vue 组件中，状态称为 props，事件称为 events，片段称为 slots。组件的构成部分也可以理解为组件对外的接口。良好的可复用组件应当定义一个清晰的公开接口。

- **Props** 允许外部环境传递数据给组件
- **Events** 允许组件触发外部环境的副作用
- **Slots** 允许外部环境将额外的内容组合在组件中。

使用 vue 对 video 组件做拓展，构造出一个支持播放列表的组件 myVideo：

```html
<my-video
  :playlist="playlist"
  width="320"
  height="240"
  @load="loadHandler"
  @error="errorHandler"
  @playnext="nextHandler"
  @playprev="prevHandler">
  <div slot="endpage"></div>
</my-video>
```

myVideo 组件有着清晰的接口，接收播放列表、播放器宽高等状态，能够触发加载成功或失败、播放上一个或下一个的事件，并且能自定义播放结束时的尾页，可用于插入广告或显示下一个视频信息。

## <a id="communication">组件状态传递</a>

## 使用自定义 watcher 优化 DOM 操作

在开发中，有些逻辑无法使用数据绑定，无法避免需要对 DOM 的操作。例如，视频的播放需要同步 Video 对象的播放操作及组件内的播放状态。可以使用自定义 watcher 来优化 DOM 的操作。

```html
<template>
  <div>
    <video ref="video" src="src"></video>
    <a
      href="javascript:;"
      @click="togglePlay">
      {{ playing ? '暂停' : '播放' }}
    </a>
  </div>
</template>

<script>
export default {
  props: {
    src: String // 播放地址
  },
  data () {
    return {
      playing: false // 是否正在播放
    }
  },
  watch: {
    // 播放状态变化时，执行对应操作
    playing (val) {
      let video = this.$refs.video
      if (val) {
        video.play();
      } else {
        video.pause();
      }
    }
  },
  method: {
    // 切换播放状态
    togglePlay () {
      this.playing = !this.playing
    }
  }
}
</script>
```

示例中，自定义 watcher 在监听到 playing 状态变化时，会执行播放或暂停操作。遇到对视频播放状态的处理时，只需要关注 playing 状态即可。
