<template>
  <div id="app">
    <JsonBeautifier />
  </div>
</template>


<script>

import JsonBeautifier from './components/JsonBeautifier.vue';

export default {
  name: 'App',
  components: {
    JsonBeautifier,
  },
};
</script>

<style>
/* Global styles */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
