<template>
  <div class="topbar">
    <drop-down-list
      :data-items="uniqueItems"
      :default-value="''"
      @change="onChange"
      :filterable="false"
    ></drop-down-list>
  </div>
</template>

<script>
import { DropDownList } from "@progress/kendo-vue-dropdowns";
// import { eventBus } from "../main.js";

export default {
  name: "MyDropDownList",
  components: {
    DropDownList
  },
  data() {
    return {
      items: []
    };
  },
  mounted() {
    var retrievedObject = localStorage.getItem("jsonServerData");
    this.items = JSON.parse(retrievedObject);
  },
  computed: {
    uniqueItems() {
      return Array.from([...new Set(this.items.map(prop => prop.name))]);
    }
  },
  // methods: {
  //   onChange(event) {
  //     console.log("MyDropDownList/onChange: " + event.target.value);
  //     eventBus.$emit("nameFilterChanged", event.target.value);
  //   }
  // }
};
</script>

<style scoped>
.topbar {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f5f5f5;
  color: #747474;
}
</style>
