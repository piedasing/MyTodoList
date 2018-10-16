<template>
  <div class="todolist" :class="{'showList': showList}">
    <p class="date">{{ date }}</p>
    <template v-for="allTodo in allTodos">
      <template v-if="true || allTodo.date === date">
        <div class="todos" v-for="(todo, index) in allTodo.todos" :key="index">
          <div class="time">{{ todo.time }}</div>
          <!-- <div class="timePic">1</div> -->
          <!-- <div class="todo" v-if="!todo.edit" @dblclick="editTodo(todo)">{{ todo.todo }}</div> -->
          <div class="todo" v-show="!todo.edit" @click="editTodo(todo)" v-html=" todo.todo"></div>
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
  // props: {
  //   date: String
  // },
  data () {
    return {
      date: '',
      showList: false,
      cacheValue: '',
      allTodos: [
        {
          date: '2018-10-13',
          todos: [
            {
              time: '00:00',
              todo: '',
              edit: false
            },
            {
              time: '01:00',
              todo: '',
              edit: false
            },
            {
              time: '02:00',
              todo: '',
              edit: false
            },
            {
              time: '03:00',
              todo: '',
              edit: false
            },
            {
              time: '04:00',
              todo: '',
              edit: false
            },
            {
              time: '05:00',
              todo: '',
              edit: false
            },
            {
              time: '06:00',
              todo: '',
              edit: false
            },
            {
              time: '07:00',
              todo: '',
              edit: false
            },
            {
              time: '08:00',
              todo: '',
              edit: false
            },
            {
              time: '09:00',
              todo: '刷牙',
              edit: false
            },
            {
              time: '10:00',
              todo: '',
              edit: false
            },
            {
              time: '11:00',
              todo: '',
              edit: false
            },
            {
              time: '12:00',
              todo: '',
              edit: false
            },
            {
              time: '13:00',
              todo: '',
              edit: false
            },
            {
              time: '14:00',
              todo: '',
              edit: false
            },
            {
              time: '15:00',
              todo: '',
              edit: false
            },
            {
              time: '16:00',
              todo: '',
              edit: false
            },
            {
              time: '17:00',
              todo: '',
              edit: false
            },
            {
              time: '18:00',
              todo: '',
              edit: false
            },
            {
              time: '19:00',
              todo: '',
              edit: false
            },
            {
              time: '20:00',
              todo: '',
              edit: false
            },
            {
              time: '21:00',
              todo: '',
              edit: false
            },
            {
              time: '22:00',
              todo: '',
              edit: false
            },
            {
              time: '23:00',
              todo: '',
              edit: false
            }
          ]
        }
      ]
    }
  },
  created () {
    const vm = this
    this.$bus.$on('showTodoList', (date) => {
      vm.showTodoList()
      vm.date = date
    })
  },
  methods: {
    showTodoList () {
      this.showList = !this.showList
    },
    editTodo (todo) {
      todo.edit = true
      this.cacheValue = todo.todo.replace(/<br>/g, '\n')
    },
    doneEdit (todo) {
      todo.edit = false
      todo.todo = this.cacheValue.replace(/\n|\r\n/g, '<br>')
      this.cacheValue = ''
    },
    cancelEdit (todo) {
      todo.edit = false
      this.cacheValue = ''
    }
  }
}
</script>

<style lang="sass">
@import 'src/assets/sass/global.sass'

.todolist
  +size(100%, 50vh)
  +center
  justify-content: flex-start
  flex-direction: column
  margin: 0 auto
  padding: 0 10%
  overflow-y: auto
  background-color: $primary
  position: absolute
  bottom: 0
  right: 0
  z-index: 999
  transform: translateX(100vw)
  transition: 0.5s
  &.showList
    transform: translateX(0)
  .date
    +size(100%, auto)
    +center
    max-height: 2rem
    background-color: #3366ff
    color: #fff
    margin: 0 auto
    padding: 1rem
  .todos
    +size(100%, auto)
    +center
    min-height: 3rem
    // max-height: 6rem
    padding: 0 1rem
    background-color: #eee
    box-shadow: 0px 0px 5px 1px #666
  .time, .timePic
    flex: none
    width: calc(100% / 3)
    +center
  .todo, .edit
    flex: auto
  .edit
    +center
    flex-direction: column
    padding: 0.5rem 1rem
    textarea
      +size(100%, auto)
      min-height: 3.5rem
      margin-bottom: 0.5rem
      resize: none
      overflow: hidden
    .btn
      +size(100%, 2rem)
      +center
      justify-content: flex-start
      button
        +size(50%, 100%)
        background-color: #3366ff
        color: #fff
        transition: 0.3s
        &:first-child
        &:last-child
          margin-left: 1rem
        &:hover
          background-color: #6633ff
</style>
