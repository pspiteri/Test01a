<script>
  import Cell from "./Cell.vue";
  export default {
    name: "row",
    components: {
      Cell
    },
    props: {
      account: Object,
      periods: Array,
      values: Array,
      selectedRowIndex: Number,
      selectedCellIndex: Number,
      isEditing: Boolean
    },
    computed: {
      rollupCss: function () {
        if (!this.account["isLeaf"]) {
          return "rollup";
        }

        return "";
      }
    }
  };
</script>
<style scoped>
  .row-header {
    text-align: left;
  }

  .rollup {
    /* background-color: #F6F6F6; */
    color: grey;
  }
</style>
<template>
  <tr v-bind:class="[rollupCss]">
    <th class="row-header" scope="row">{{ account["name"] }}</th>
    <template v-for="period in periods">
      <Cell :selectedRowIndex="selectedRowIndex" :selectedCellIndex="selectedCellIndex" :account="account"
        :period="period" :invertSign=false :values="values" :isEditing="isEditing" v-on="$listeners" />
    </template>
  </tr>
</template>