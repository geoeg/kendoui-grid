<template>
  <div id="my-grid">
    <div id="selected-ttem">
      Selected Item: ID: {{ selectedItem && selectedItem.ProductID }}, Name:
      {{ selectedItem && selectedItem.ProductName }}, Unit Price:
      {{ selectedItem && selectedItem.UnitPrice }}
    </div>
    <hr>
    <grid
      :style="{ height: 'auto' }"
      :data-items="pageItems"
      :columns="columns"
      :selected-field="selectedField"
      @rowclick="onRowClick"
      :pageable="true"
      :skip="skippedItems"
      :take="singlePageItems"
      :total="totalItems"
      @pagechange="pageChangeHandler"
    ></grid>
  </div>
</template>

<script>
import { Grid } from "@progress/kendo-vue-grid";

export default {
  name: "MyGrid",
  components: {
    Grid
  },
  data: function() {
    return {
      columns: [
        { field: "ProductID", title: "Product ID" },
        { field: "ProductName", title: "Product Name" },
        { field: "UnitPrice", title: "Unit Price" }
      ],
      items: [],
      selectedField: "selected",
      selectedID: 1,
      skippedItems: 0,
      singlePageItems: 10
    };
  },
  methods: {
    createRandomData(count) {
      const productNames = [
        "Chai",
        "Chang",
        "Syrup",
        "Apple",
        "Orange",
        "Banana",
        "Lemon",
        "Pineapple",
        "Tea",
        "Milk"
      ];
      const unitPrices = [12.5, 10.1, 5.3, 7, 22.53, 16.22, 20, 50, 100, 120];
      return Array(count)
        .fill({})
        .map((_, idx) => ({
          ProductID: idx + 1,
          ProductName:
            productNames[Math.floor(Math.random() * productNames.length)],
          UnitPrice: unitPrices[Math.floor(Math.random() * unitPrices.length)]
        }));
    },
    onRowClick(event) {
      this.selectedID = event.dataItem.ProductID;
    },
    pageChangeHandler: function(event) {
      this.skippedItems = event.page.skip;
      this.singlePageItems = event.page.take;
    }
  },
  mounted() {
    this.items = this.createRandomData(50);
  },
  computed: {
    selectedItem() {
      return this.items.find(item => item.ProductID == this.selectedID);
    },
    totalItems() {
      return this.items ? this.items.length : 0;
    },
    pageItems: {
      get: function() {
        return this.items.slice(
          this.skippedItems,
          this.singlePageItems + this.skippedItems
        );
      }
    }
  }
};
</script>

<style scoped></style>
