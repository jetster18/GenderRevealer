<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <gender-revealer></gender-revealer>

  </div>
</template>

<script>
import GenderRevealer from './components/GenderRevealer.vue'


export default {
  name: 'App',
  components: {
    GenderRevealer
  },
  data(){
    return {
     
    }
  },
  methods:{
    toggleSound(){
      this.isMuted = !this.isMuted;
       document.getElementById('audio').play();
    }
  },
  mounted(){
    // document.getElementById('audio').muted = true;
    // document.getElementById('audio').play();
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

body{
  background-color: lightblue;
}
</style>
