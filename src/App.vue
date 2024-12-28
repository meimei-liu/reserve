<template>
  <div id="app">
      <Reserve />
  </div>
</template>

<script>
import Reserve from './components/reserve.vue';

export default {
  name: 'App',
  components: {
    Reserve
  }
};
</script>

<style scoped>
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  padding: 20px;
}

</style>