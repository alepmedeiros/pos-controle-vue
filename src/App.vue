<template>
  <div id="app">
    <AppNavbar />
    <POS />
  </div>
</template>

<script>
import AppNavbar from './components/AppNavbar.vue';
import POS from './components/POS.vue';

export default {
  name: 'App',
  components: {
    AppNavbar,
    POS
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
