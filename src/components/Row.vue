<script>
  import Cell from "./Cell.vue";
  export default {
    name: "row",
    components: {
      Cell
    },
    props: {
      account: String,
      periods: Array,
      values: Array,
      selectedRowIndex: Number,
      selectedCellIndex: Number
    }
  };
</script>
<style scoped>
  .row-header {
    text-align: left;
  }
</style>
<template>
  <tr>
    <th class="row-header" scope="account">{{ account }}</th>
    <template v-for="period in periods">
      <Cell :selectedRowIndex="selectedRowIndex" :selectedCellIndex="selectedCellIndex" :account="account"
        :period="period['name']" :invertSign=false :values="values" v-on="$listeners" />
    </template>
  </tr>
</template>