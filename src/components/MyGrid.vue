<template>
  <div id="my-grid">
    <div id="selected-ttem">
      Selected Item: ID: {{ selectedItem && selectedItem.ProductID }}, Name:
      {{ selectedItem && selectedItem.ProductName }}, Unit Price:
      {{ selectedItem && selectedItem.UnitPrice }}
    </div>
    <grid
      :style="{ height: 'auto' }"
      :data-items="pageItems"
      :columns="columns"
      :selected-field="selectedField"
      @rowclick="onRowClick"
      :pageable="true"
      :skip="skip"
      :take="take"
      :total="total"
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
      skip: 0,
      take: 10
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
      this.skip = event.page.skip;
      this.take = event.page.take;
    }
  },
  mounted() {
    this.items = this.createRandomData(50);
  },
  computed: {
    selectedItem() {
      return this.items.find(item => item.ProductID == this.selectedID);
    },
    total() {
      return this.items ? this.items.length : 0;
    },
    pageItems: {
      get: function() {
        return this.items.slice(this.skip, this.take + this.skip);
      }
    }
  }
};
</script>

<style scoped>
.restore-columns {
  margin: 0 0 20px;
  padding: 20px;
  background-color: rgba(0, 0, 0, 0.03);
  border: 1px solid rgba(0, 0, 0, 0.08);
}
</style>
