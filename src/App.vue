<template>
  <div id="app">
      <my-header></my-header>
    <router-view></router-view>
  </div>
</template>

<script>
import myHeader from './component/header.vue'

  export default {
    name: 'app',
    data() {
      return {
        msg: 'hello ！welcome to Tangless~VUE',
      }
    },
    components: {myHeader},
    created: function () {
      this.$router.push('/index/shili')//刷新进入index页
    }
  }
</script>
