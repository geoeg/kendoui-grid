<template>
  <div id="app">
    <!-- <my-drop-down-list :dropDownOptions="columns" /> -->
    <my-grid :columns="columns" :dbData="jsonServerData"></my-grid>
  </div>
</template>

<script>
import "@progress/kendo-theme-default/dist/all.css";
// import MyDropDownList from "./components/MyDropDownList.vue";
import MyGrid from "./components/MyGrid.vue";
import axios from 'axios';

const baseURL = "http://localhost:3000/products"

export default {
  name: "App",
  components: {
    // MyDropDownList,
    MyGrid
  },
  data: function() {
    return {
      columns: [
        { field: "id", title: "Product ID", headerCell: "tableHeader", filterable: false },
        { field: "name", title: "Product Name", headerCell: "tableHeader" },
        { field: "unitPrice", title: "Unit Price", headerCell: "tableHeader", filter: "numeric" }
      ],
      jsonServerData: []
    };
  },
  async created() {
    try {
      const res = await axios.get(baseURL)
      this.jsonServerData = res.data;
      localStorage.setItem('jsonServerData', JSON.stringify(this.jsonServerData));
    } catch(e) {
      console.error(e)
    }
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
