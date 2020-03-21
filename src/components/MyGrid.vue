<template>
  <div id="my-grid">
    <span>
      <button @click="restoreColumns" class="k-button k-primary">
        Restore hidden columns
      </button>
    </span>
    <span>
      Selected Item: ID: {{ selectedItem && selectedItem.ProductID }}, 
      Name: {{ selectedItem && selectedItem.ProductName }}, 
      Unit Price: {{ selectedItem && selectedItem.UnitPrice }}
    </span>
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
    >
      <template v-slot:tableHeader="{ props }">
        <span>
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

export default {
  name: "MyGrid",
  components: {
    Grid
  },
  data: function() {
    return {
      columns: [
        { field: "ProductID", title: "Product ID", headerCell: "tableHeader" },
        {
          field: "ProductName",
          title: "Product Name",
          headerCell: "tableHeader"
        },
        { field: "UnitPrice", title: "Unit Price", headerCell: "tableHeader" }
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
    },
    hideColumn: function(e) {
      this.columns.map(column => {
        if (column.field === e) {
          Vue.set(column, "hidden", true);
        }
      });
    },
    restoreColumns: function() {
      this.columns.map(function(column) {
        if (column.hidden) {
          Vue.set(column, "hidden", false);
        }
      });
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
