<template>
  <div id="app">
    <Header />
    <CovidData />
  </div>
</template>

<script>
import Header from './components/Header'
import CovidData from './components/CovidData'

export default {
  name: 'App',
  components: {
    Header,
    CovidData
  }
}

</script>

<style scope>
#app {
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
