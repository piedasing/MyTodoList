<template>
  <div id="app">
    <div class="nav">
      <div class="logo">我的行事曆</div>
      <div class="navlink">
        <router-link to="/">Home</router-link>
        <!-- <router-link to="/calendar">行事曆</router-link> -->
      </div>
    </div>
    <router-view/>
  </div>
</template>

<style lang="sass">
@import 'src/assets/sass/global.sass'

#app
  +size(100%)
  font-family: 'Microsoft JhengHei'
  .nav
    +size(100%, 100px)
    +center
    justify-content: flex-start
    background-color: rgba(78, 208, 147, 0.6)
    padding: 0 10%
    .logo
      height: 100%
      +center
      justify-content: flex-start
      font-size: 1.6rem
      font-weight: bold
      color: #51687A
      padding: 0 1rem
      border: 1px solid #51687A
      border-radius: 0rem 5rem
    .navlink
      height: 100%
      +center
      a
        height: 60%
        +center
        margin: 0 1rem
        padding: 1rem
        border-radius: 1rem
        color: #40AA5F
        &:hover
          color: #FFF
          background-color: #40AA5F
</style>
