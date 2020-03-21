<template>
  <div id="app">
    <my-drop-down-list />
    <my-grid></my-grid>
  </div>
</template>

<script>
import "@progress/kendo-theme-default/dist/all.css";
import MyDropDownList from "./components/MyDropDownList.vue";
import MyGrid from "./components/MyGrid.vue";

export default {
  name: "App",
  components: {
    MyDropDownList,
    MyGrid
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
