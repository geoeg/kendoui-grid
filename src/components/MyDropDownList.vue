<template>
  <div class="my-drop-down-list">
    <span>Filter by: </span>
    <drop-down-list
      :data-items="options"
      :default-value="''"
    ></drop-down-list>
  </div>
</template>

<script>
import { DropDownList } from "@progress/kendo-vue-dropdowns";

export default {
  name: "MyDropDownList",
  components: {
    DropDownList
  },
  props: ["dropDownOptions"],
  data() {
    return {
      options: this.dropDownOptions.map(prop => prop.title),
      
    };
  }
};
</script>

<style scoped></style>
