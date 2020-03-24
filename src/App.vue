<template>
  <div id="app">
    <my-grid></my-grid>
  </div>
</template>

<script>
import "@progress/kendo-theme-default/dist/all.css";
import MyGrid from "./components/MyGrid.vue";

export default {
  name: "App",
  components: {
    MyGrid
  },
  data: function() {
    return {};
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
