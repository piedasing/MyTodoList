<template>
  <div class="todolist">
    <template v-for="allTodo in allTodos">
      <template v-if="allTodo.date === date">
        <div class="todos" v-for="(todo, index) in allTodo.todos" :key="index">
          <div class="time">{{ todo.time }}</div>
          <div class="timePic">1</div>
          <!-- <div class="todo" v-if="!todo.edit" @dblclick="editTodo(todo)">{{ todo.todo }}</div> -->
          <div class="todo" v-show="!todo.edit" @dblclick="editTodo(todo)" v-html=" todo.todo"></div>
          <div class="edit" v-show="todo.edit">
            <textarea v-model.trim="cacheValue"></textarea>
            <div class="btn">
              <button @click="doneEdit(todo)">確認</button>
              <button @click="cancelEdit(todo)">取消</button>
            </div>
          </div>
        </div>
      </template>
    </template>
  </div>
</template>

<script>
export default {
  name: 'todolist',
  props: {
    date: String,
  },
  data () {
    return {
      cacheValue: '',
      allTodos: [
        {
          date: '2018-10-13',
          todos: [
            {
              time: '09:00',
              todo: '刷牙',
              edit: false,
            },
          ],
        },
      ],
    };
  },
  methods: {
    editTodo(todo) {
      todo.edit = true;
      this.cacheValue = todo.todo.replace(/<br>/g, "\n");
    },
    doneEdit(todo) {
      todo.edit = false;
      todo.todo = this.cacheValue.replace(/\n|\r\n/g, "<br>");
      this.cacheValue = '';
    },
    cancelEdit(todo) {
      todo.edit = false;
      this.cacheValue = '';
    },
  },
};
</script>

<style lang="sass">
@import 'src/assets/sass/global.sass'

.todolist
  +size(80%, 75vh)
  +center
  align-items: flex-start
  margin: 0 auto
  overflow-y: auto
  background-color: #ccc
  .todos
    +size(100%, auto)
    +center
    min-height: 3rem
    // max-height: 6rem
    overflow-y: auto
    background-color: #eee
  .time, .timePic
    flex: none
    width: 10vw
  .todo, .edit
    flex: auto
  .edit
    +center
    flex-direction: column
    padding: 0.5rem 1rem
    textarea
      +size(100%, auto)
      min-height: 2.5rem
      margin-bottom: 0.5rem
      resize: none
      overflow: hidden
    .btn
      +size(100%, 1.5rem)
      +center
      justify-content: flex-start
      button
        +size(8vw, 100%)
        &:first-child
        &:last-child
          margin-left: 1rem

</style>
