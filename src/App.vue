<template>
  <div id="app">
      <my-header :child-com="content" @showbox="toshow"></my-header>
      <!--<input v-model="content" />-->
    <router-view></router-view>
  </div>
</template>

<script>
import myHeader from './component/header.vue'

  export default {
    name: 'app',
    data() {
      return {
        content: 'hello ！welcome to Tangless~VUE',
      }
    },
    components: {myHeader},
    created: function () {
      this.$router.push('/index/shili')//刷新进入index页
    },
    methods:{
      toshow:function(msg){
        this.content=msg
      }
    }
  }
</script>
