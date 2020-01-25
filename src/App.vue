<template>
  <div id="app">
    <Main/>
  </div>
</template>

<script>
import Main from './components/Main.vue'

export default {
  name: 'app',
  components: {
    Main
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Poppins:400,700,900&display=swap');

  body {
    padding: 0;
    margin: 0;
  }
  #app {
    background-color: #cecece;
    width: 100%;
    height: 100vh;
    font-family: poppins;
  }
  ul {
    list-style-type: none;
  }
</style>
