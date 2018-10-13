<template>
  <div class="home">
    <Calendar />
    <TodoList :date="'2018-10-13'" />
  </div>
</template>

<script>
// @ is an alias to /src
import Calendar from '@/components/Calendar.vue';
import TodoList from '@/components/TodoList.vue';

export default {
  name: 'home',
  components: {
    Calendar, TodoList,
  },
};
</script>
