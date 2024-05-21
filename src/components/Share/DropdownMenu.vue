<template>
  <div :class="{active:isActive}" class="share-dropdown-menu">
    <div class="share-dropdown-menu-wrapper">
      <span class="share-dropdown-menu-title" @click.self="clickTitle">{{ title }}</span>
      <div v-for="(item,index) of items" :key="index" class="share-dropdown-menu-item">
        <a v-if="item.href" :href="item.href" target="_blank">{{ item.title }}</a>
        <span v-else>{{ item.title }}</span>
      </div>
    </div>
  </div>
</template>

<script>
/*
* @click.native 原生点击事件
* 1、给vue组件磅蛋事件时候，必须加上native,不然不会生效（监听根元素的原生事件，使用.native修饰符
* 2、等同于在自组件中：子组件内部处理click事件然后向外发送click事件：$emit('click', fn)
*  阻止单击事件冒泡
* <a v-on:click.stop="click"></a>
* 提交事件不再重载页面
* <form v-on:submit.prevent="submit"></form>
* 修饰符可以串联
* <a v-on.click.stop.prevent="doThat"></a>
* 元素自身触发的事件先在此处理，然后才交由内部元素进行处理
* <div v-on:click.capture="doThis"></div>
*<!-- 即事件不是从内部元素触发的 -->
*<div v-on:click.self="doThat">...</div>
* @click.prevent.self 和 @click.self.prevent
使用修饰符时需要注意调用顺序，因为相关代码是以相同的顺序生成的
@click.prevent.self 会阻止元素及其子元素的所有点击事件的默认行为，而 @click.self.prevent 则只会阻止对元素本身的点击事件的默认行为。
* */
export default {
  props: {
    items: {
      type: Array,
      default: function() {
        return []
      }
    },
    title: {
      type: String,
      default: 'vue'
    }
  },
  data() {
    return {
      isActive: false
    }
  },
  methods: {
    clickTitle() {
      this.isActive = !this.isActive
    }
  }
}
</script>

<style lang="scss" >
$n: 9; //和items.length 相同
$t: .1s;
.share-dropdown-menu {
  width: 250px;
  position: relative;
  z-index: 1;
  height: auto!important;
  &-title {
    width: 100%;
    display: block;
    cursor: pointer;
    background: black;
    color: white;
    height: 60px;
    line-height: 60px;
    font-size: 20px;
    text-align: center;
    z-index: 2;
    transform: translate3d(0,0,0);
  }
  &-wrapper {
    position: relative;
  }
  &-item {
    text-align: center;
    position: absolute;
    width: 100%;
    background: #e0e0e0;
    color: #000;
    line-height: 60px;
    height: 60px;
    cursor: pointer;
    font-size: 18px;
    overflow: hidden;
    opacity: 1;
    transition: transform 0.28s ease;
    &:hover {
      background: black;
      color: white;
    }
    @for $i from 1 through $n {
      &:nth-of-type(#{$i}) {
        z-index: -1;
        transition-delay: $i*$t;
        transform: translate3d(0, -60px, 0);
      }
    }
  }
  &.active {
    .share-dropdown-menu-wrapper {
      z-index: 1;
    }
    .share-dropdown-menu-item {
      // 获取数组 $i 对应的下标值
      @for $i from 1 through $n {
        &:nth-of-type(#{$i}) {
          transition-delay: ($n - $i)*$t;
          transform: translate3d(0, ($i - 1)*60px, 0);
        }
      }
    }
  }
}
/**
  scss-@for指令
  此指令用于循环输出，具有两种循环方式
  1、.@for $var from <start> through <end>  该方式遍历索引区间是[start,end]
  2、 .@for $var from <start> to <end> 该方式的遍历索引区间是[start, end-1]
*/
</style>
