<template>
  <div id="my-grid">
    <div class="topbar k-button k-primary">
      <div v-if="selectedID == 0">No item selected.</div>
      <div v-else>
        Selected Item: ID: {{ selectedItem && selectedItem.ProductID }}, Name:
        {{ selectedItem && selectedItem.ProductName }}, Unit Price:
        {{ selectedItem && selectedItem.UnitPrice }}
      </div>
      <button @click="restoreColumns" class="k-button k-primary">
        Restore hidden columns
      </button>
    </div>
    <grid
      :style="{ height: 'auto' }"
      :data-items="filteredItems"
      :columns="columns"
      :selected-field="selectedField"
      @rowclick="onRowClick"
      :pageable="true"
      :skip="skippedItems"
      :take="singlePageItems"
      :total="totalItems"
      @pagechange="pageChangeHandler"
      :filterable="true"
      :filter="filter"
      @filterchange="filterChange"
      :scrollable="'none'"
    >
      <template v-slot:tableHeader="{ props }">
        <span class="column-headers">
          <span>{{ props.title }}</span>
          <button
            @click="hideColumn(props.field)"
            class="k-button k-primary"
            style="float: right;"
          >
            Hide
          </button>
        </span>
      </template>
    </grid>
  </div>
</template>

<script>
import Vue from "vue";
import { Grid } from "@progress/kendo-vue-grid";
import { filterBy } from "@progress/kendo-data-query";

export default {
  name: "MyGrid",
  components: {
    Grid
  },
  props: ["columns"],
  data: function() {
    return {
      items: [],
      selectedField: "selected",
      selectedID: 0,
      skippedItems: 0,
      singlePageItems: 10,
      filter: {
        logic: "and",
        filters: [
          { field: "ProductName", operator: "gte", value: "" },
          { field: "UnitPrice", operator: "gte", value: 0 }
        ]
      }
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
    pageChangeHandler(event) {
      this.skippedItems = event.page.skip;
      this.singlePageItems = event.page.take;
    },
    hideColumn(event) {
      this.columns.map(column => {
        if (column.field === event) {
          Vue.set(column, "hidden", true);
        }
      });
    },
    restoreColumns() {
      this.columns.map(column => {
        if (column.hidden) {
          Vue.set(column, "hidden", false);
        }
      });
    },
    filterChange(event) {
      this.filter = event.filter;
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
    },
    filteredItems() {
      return filterBy(this.pageItems, this.filter);
    }
  }
};
</script>

<style scoped>
.topbar,
.column-headers {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
