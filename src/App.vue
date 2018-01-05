<template>
  <div id="app">
    <Content/>
  </div>
</template>

<script>
import Content from './components/Content'

export default {
  name: 'app',
  components: {
    Content
  }
}
</script>

<style>
  html, body {
    height: 100%;
    width: 100%;
  }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;

    align-items: center;
    display: flex;
    justify-content: center;
    height: 100%;
    width: 100%;
  }
</style>
