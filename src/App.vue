<template>
  <div id="app">
    <img alt="Vue logo" width="100px" src="https://2.bp.blogspot.com/--BYLyPduBgM/WaPvldUDt9I/AAAAAAABGNw/XTkwA_fBEC86bpvy2z7-1jZ9-dEc_6_jQCLcBGAs/s800/dentaku_syoumen_small.png
">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <calculator />
  </div>
</template>
<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
