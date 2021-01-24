<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <input-tags />
  </div>
</template>

<script>
import InputTags from './components/Tags.vue'

export default {
  name: 'App',
  components: {
		InputTags
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
* {
	box-sizing: border-box;
}
</style>
