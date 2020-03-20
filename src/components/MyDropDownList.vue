<template>
  <div class="my-drop-down-list">
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
      sports: ["Baseball", "Cricket", "Field Hockey", "Football", "Volleyball"]
    };
  }
};
</script>

<style scoped></style>
