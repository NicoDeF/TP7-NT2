<template>
<header>
<Navbar />
</header>
<br>
<router-view/>
</template>

<script>
import  Navbar from './components/Navbar.vue'

export default {
  name: 'App',
  components: {
    Navbar
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
header {
    width: 100%;
    position: absolute;
    top: 0;
    text-align: center;
}

</style>
