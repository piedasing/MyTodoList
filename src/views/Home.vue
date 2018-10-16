<template>
  <div class="home" v-on:click="closeTodo">
    <Calendar />
    <TodoList />
  </div>
</template>

<script>
// @ is an alias to /src
import Calendar from '@/components/Calendar.vue'
import TodoList from '@/components/TodoList.vue'

export default {
  name: 'home',
  components: {
    Calendar, TodoList
  },
  methods: {
    closeTodo () {
      // this.$bus.$emit('showTodoList')
    }
  }
}
</script>
