<template>
	<div id="app">
		<Header/>
		<Home/>
	</div>
</template>

<script>
import Header from './components/Header.vue'
import Home from './components/Home.vue'

export default {
	name: 'App',
	components: {
		Header,
		Home
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
  margin: 0px;
  background-color: blue;
  height: 100vh;
  background-image: url("./assets/sky.png");
  background-size: cover;
}
</style>
