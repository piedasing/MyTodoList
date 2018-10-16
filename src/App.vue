<template>
  <div id="app">
    <div class="nav-md">
      <div class="logo">行事曆</div>
      <div class="navlink">
        <router-link to="/">Home</router-link>
        <!-- <router-link to="/calendar">行事曆</router-link> -->
      </div>
    </div>
    <div class="nav-sm" :class="{'showMenu': showMenu}">
      <div class="menu_btn" v-on:click="showMenu = !showMenu">
        <i class="fas fa-bars"></i>
      </div>
      <div class="menu">
        <a href="javascript:;" class="close"></a>
        <div class="logo">行事曆</div>
        <div class="navlink">
          <router-link to="/">Home</router-link>
          <!-- <router-link to="/calendar">行事曆</router-link> -->
        </div>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      showMenu: false
    }
  }
}
</script>

<style lang="sass">
@import 'src/assets/sass/global.sass'

#app
  +size(100%)
  font-family: 'Microsoft JhengHei'
  overflow: hidden
  position: relative
  .nav-md
    +size(100%, 100px)
    +center
    justify-content: flex-start
    background-color: $primary
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
  .nav-sm
    +size(40vw, 100%)
    display: flex
    justify-content: flex-start
    align-items: flex-start
    flex-direction: column
    position: absolute
    top: 0
    left: 0
    // background-color: rgb(78, 208, 147)
    background-color: $primary
    transform: translateX(-40vw)
    transition: 0.5s
    .menu_btn
      padding: 10px 20px
      position: absolute
      top: 0px
      left: calc(40vw)
    &.showMenu
      transform: translateX(0)
    .logo
      width: 100%
      +center
      font-size: 1.6rem
      font-weight: bold
      color: #51687A
      padding: 0 1rem
      border: 1px solid #51687A
      border-radius: 0rem 5rem

@media screen and (max-width: 767px)
  .nav-md
    display: none !important
  .nav-sm
    display: flex !important
@media screen and (min-width: 768px)
  .nav-md
    display: flex !important
  .nav-sm
    display: none !important
</style>
