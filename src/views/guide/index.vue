<template>
  <div class="app-container">
    <aside>
      The guide page is useful for some people who entered the project for the first time. You can briefly introduce the
      features of the project. Demo is based on
      <a href="https://github.com/kamranahmedse/driver.js" target="_blank">driver.js.</a>
    </aside>
    <!--aside标签-->
    <el-button icon="el-icon-question" type="primary" @click.prevent.stop="guide">
      Show Guide
    </el-button>
  </div>
</template>

<script>
import Driver from 'driver.js' // import driver.js
import 'driver.js/dist/driver.min.css' // import driver.js css
import steps from './steps'

export default {
  name: 'Guide',
  data() {
    return {
      driver: null
    }
  },
  mounted() {
    this.driver = new Driver()
    // this.driver = new Driver({
    //   prevBtnText: '上一步',
    //   nextBtnText: '下一步',
    //   doneBtnText: '完成',
    //   closeBtnText: '关闭',
    //   allowClose: Boolean,
    //   animate: '动画',
    //   opacity: 0.5, // 遮罩层
    //   padding: 10,
    //   keyboardControl: false, // 禁止键盘操作
    //   onHighlightStarted: (e) => {
    //     //样式
    //   }
    // })
  },
  methods: {
    // 点击操作指引
    guide() {
      this.driver.defineSteps(steps)
      this.driver.start()
    }
  }
}
</script>
