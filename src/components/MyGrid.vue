<template>
  <div class="my-grid">
    <div class="topbar">
      <div v-if="selectedID == 0">No item selected.</div>
      <div v-else>
        Selected Item: ID: {{ selectedItem.id }}, 
        Name: {{ selectedItem.name }},
        Unit Price: {{ selectedItem.unitPrice }}
      </div>
      <button @click="restoreColumns" class="k-button k-primary">
        Restore hidden columns
      </button>
    </div>
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
      :filterable="false"
      :filter="filter"
      @filterchange="filterChange"
      :scrollable="'none'"
    >
      <template v-slot:tableHeader="{ props }">
        <span class="column-headers">
          <span>{{ props.title }}</span>
          <span v-if="columns.filter(obj => obj.title == props.title)[0].filterable !== false">
            <my-drop-down-list
              :dropDownListData="items" 
              :dropDownType="props.field"
            ></my-drop-down-list>
          </span>
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
import MyDropDownList from "./MyDropDownList.vue";
import { eventBus } from "../main.js";

export default {
  name: "MyGrid",
  components: {
    Grid,
    MyDropDownList
  },
  data: function() {
    return {
      items: [],
      columns: [
        { field: "id",  title: "Product ID", headerCell: "tableHeader", filterable: false },
        { field: "name", title: "Product Name", headerCell: "tableHeader" },
        { field: "unitPrice", title: "Unit Price", headerCell: "tableHeader",filter: "numeric" }
      ],
      selectedField: "selected",
      selectedID: 0,
      skippedItems: 0,
      singlePageItems: 10,
      filter: {
        logic: "and",
        filters: [
          { field: "name", operator: "contains", value: "" },
          { field: "unitPrice", operator: "gte", value: 0 }
        ]
      },
      dropDownFilter: ""
    };
  },
  methods: {
    onRowClick(event) {
      this.selectedID = event.dataItem.id;
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
    },
    dropDownFilterChange() {
      eventBus.$on("filterWasChanged", value => {
        this.dropDownFilter = value;      
        this.filter.filters.filter(obj => obj.field == value[1])[0].value = value[0];
      });
    }
},
  mounted() {
    this.items = [
      {
        id: 1,
        name: "Apple",
        unitPrice: 5
      },
      {
        id: 2,
        name: "Orange",
        unitPrice: 10
      },
      {
        id: 3,
        name: "Lemon",
        unitPrice: 15
      },
      {
        id: 4,
        name: "Orange",
        unitPrice: 10
      },
      {
        id: 5,
        name: "Lemon",
        unitPrice: 15
      },
      {
        id: 6,
        name: "Apple",
        unitPrice: 5
      },
      {
        id: 7,
        name: "Lemon",
        unitPrice: 15
      },
      {
        id: 8,
        name: "Orange",
        unitPrice: 10
      },
      {
        id: 9,
        name: "Apple",
        unitPrice: 5
      },
      {
        id: 10,
        name: "Orange",
        unitPrice: 100
      },
      {
        id: 11,
        name: "Apple",
        unitPrice: 50
      },
      {
        id: 12,
        name: "Orange",
        unitPrice: 100
      },
      {
        id: 13,
        name: "Lemon",
        unitPrice: 150
      },
      {
        id: 14,
        name: "Orange",
        unitPrice: 100
      },
      {
        id: 15,
        name: "Lemon",
        unitPrice: 150
      },
      {
        id: 16,
        name: "Apple",
        unitPrice: 50
      },
      {
        id: 17,
        name: "Orange",
        unitPrice: 100
      },
      {
        id: 18,
        name: "Lemon",
        unitPrice: 150
      },
      {
        id: 19,
        name: "Apple",
        unitPrice: 50
      },
      {
        id: 20,
        name: "Orange",
        unitPrice: 100
      },
      {
        id: 21,
        name: "Apple",
        unitPrice: 5
      },
      {
        id: 22,
        name: "Pineapple",
        unitPrice: 500
      }
    ];
    this.dropDownFilterChange();
  },
  computed: {
    selectedItem() {
      return this.items.find(item => item.id == this.selectedID);
    },
    totalItems() {
      return this.items ? this.items.length : 0;
    },
    pageItems: {
      get() {
        let itemsN = this.filteredItems.slice(
          this.skippedItems,
          this.singlePageItems + this.skippedItems
        );
        return itemsN;
      }
    },
    filteredItems: {
      get() {
        return filterBy(this.items, this.filter);
      }
    }
  },
};
</script>

<style scoped>
.my-grid {
  color:#747474;
}

.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.column-headers {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
