<template>
    <drop-down-list
      :data-items="uniqueValues"
      :default-value="''"
      @change="onChange"
      :filterable="false"
    ></drop-down-list>
</template>

<script>
import { DropDownList } from "@progress/kendo-vue-dropdowns";
import { eventBus } from "../main.js";

export default {
  name: "MyDropDownList",
  components: {
    DropDownList
  },
  data() {
    return {
      // items: []
    };
  },
  props: ["dropDownListData", "dropDownType"],
  computed: {
    uniqueValues() {
      let itemsList = Array.from([...new Set(this.dropDownListData.map(prop => prop[this.dropDownType]))]);
      itemsList.unshift("");
      return itemsList;
    }
  },
  methods: {
    onChange(event) {
      eventBus.$emit("filterWasChanged", ([event.target.value, this.dropDownType]));
    }
  }
};
</script>

<style scoped></style>
