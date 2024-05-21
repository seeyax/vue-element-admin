<template>
  <li :class="{ completed: todo.done, editing: editing }" class="todo">
    <div class="view">
      <input
        :checked="todo.done"
        class="toggle"
        type="checkbox"
        @change="toggleTodo( todo)"
      >
      <label @dblclick="editing = true" v-text="todo.text" />
      <button class="destroy" @click="deleteTodo( todo )" />
    </div>
    <input
      v-show="editing"
      v-focus="editing"
      :value="todo.text"
      class="edit"
      @keyup.enter="doneEdit"
      @keyup.esc="cancelEdit"
      @blur="doneEdit"
    >
  </li>
</template>

<script>
export default {
  name: 'Todo',
  /** *
   *  1.在生命周期内
   *  focus: function (el, binding,vnode) {
   *    el: 可以获取当前dom节点,并且进行编译，也可以操作事件
   *    binding: 指的是一个对象，一般不用
   *    vnode: 是Vue编译生成的虚拟节点
   *  }
   * 2.自定义指令 eg:
   * const has = Vue.directive('has', {
   *   bind: function (el, binding,vnode) {
   *     ...
   *   }
   * })
   */
  directives: {
    focus(el, { value }, { context }) {
      if (value) {
        context.$nextTick(() => {
          el.focus()
        })
      }
    }
  },
  props: {
    todo: {
      type: Object,
      default: function() {
        return {}
      }
    }
  },
  data() {
    return {
      editing: false
    }
  },
  methods: {
    deleteTodo(todo) {
      this.$emit('deleteTodo', todo)
    },
    editTodo({ todo, value }) {
      this.$emit('editTodo', { todo, value })
    },
    toggleTodo(todo) {
      this.$emit('toggleTodo', todo)
    },
    doneEdit(e) {
      const value = e.target.value.trim()
      const { todo } = this
      if (!value) {
        this.deleteTodo({
          todo
        })
      } else if (this.editing) {
        this.editTodo({
          todo,
          value
        })
        this.editing = false
      }
    },
    cancelEdit(e) {
      e.target.value = this.todo.text
      this.editing = false
    }
  }
}
</script>
