<template>
  <div class="my-drop-down-list">
    <h1>{{ msg }}</h1>
    <drop-down-list
      :data-items="sports"
      :default-value="'Baseball'"
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
  data() {
    return {
      sports: ["Baseball", "Cricket", "Field Hockey", "Football", "Volleyball"],
      msg: "Hello Kendo Native DropDownList for Vue.js"
    };
  }
};
</script>

<style scoped></style>
