<template>
  <div id="app">
    <table>
      <tr>
        <th @click="sort('assetClass')">Sort by Asset Class</th>
        <th @click="sort('price')">Sort by Price</th>
        <th @click="sort('ticker')">Sort by Ticker</th>
      </tr>

      <tr
        v-for="(item, i) in sortedItems"
        :key="i + 'A'"
        v-bind:class="item.assetClass"
      >
        <td>{{ item.assetClass }}</td>
        <td :class="{ positive: item.price > 0, negative: item.price <= 0 }">
          {{ item.price }}
        </td>
        <td>{{ item.ticker }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

Vue.use(VueAxios, axios);

export default {
  name: "App",

  data() {
    return {
      items: [],
      currentSort: "Asset Class",
      currentSortDir: "asc",
    };
  },

  created() {
    var _this = this;
    axios
      .get("http://localhost:8080/sampleData.json")
      .then((response) => {
        _this.items = response.data;
      })
      .catch(function(error) {
        console.log(error);
      });
  },

  methods: {
    sort: function(s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = s;
    },
  },
  computed: {
    sortedItems: function() {
      return [...this.items].sort((a, b) => {
        let modifier = 1;
        if (this.currentSortDir === "desc") modifier = -1;
        if (a[this.currentSort] < b[this.currentSort]) return 1 * modifier;
        if (a[this.currentSort] > b[this.currentSort]) return -1 * modifier;
        return 0;
      });
    },
  },
};
</script>

<style>
table {
  width: 100%;
  border-collapse: collapse;
}

th,
tr,
td {
  text-align: left;
  border: 1px solid #ccc;
  padding: 10px;
}

th {
  background-color: #333;
  color: white;
}

th:hover {
  cursor: pointer;
  background-color: black;
}

tr.Macro {
  background-color: white;
}

tr.Equities {
  background-color: lightsteelblue;
}

tr.Credit {
  background-color: lightseagreen;
}

td.negative {
  color: red;
}

td.positive {
  color: blue;
}
</style>
