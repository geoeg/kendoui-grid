<template>
  <div id="app">
    <!-- <my-drop-down-list :dropDownOptions="columns" /> -->
    <my-grid :columns="columns"></my-grid>
  </div>
</template>

<script>
import "@progress/kendo-theme-default/dist/all.css";
// import MyDropDownList from "./components/MyDropDownList.vue";
import MyGrid from "./components/MyGrid.vue";

export default {
  name: "App",
  components: {
    // MyDropDownList,
    MyGrid
  },
  data: function() {
    return {
      columns: [
        { field: "ProductID", title: "Product ID", headerCell: "tableHeader", filterable: false },
        { field: "ProductName", title: "Product Name", headerCell: "tableHeader" },
        { field: "UnitPrice", title: "Unit Price", headerCell: "tableHeader", filter: "numeric" }
      ]
    };
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
