<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "FinancialTable",
  props: {
    msg: String
  },

  computed: {
    APIURL: function() {
      return `sampleData.json`;
    }
  },

  created() {
    axios.get(this.APIURL).then(response => {});
  }
};
</script>
