<script>
    // https://dev-notes.eu/2018/05/passing-data-between-vue-components/
    // https://laracasts.com/discuss/channels/vue/computed-function-is-running-before-data-is-loaded?page=0

    var numeral = require("numeral");
    // import Row from "./Row.vue";
    export default {
        name: "cell",
        data() {
            return {
                componentLoaded: false
            }
        },
        props: {
            account: String,
            period: String,
            invertSign: Boolean,
            values: Array,
            selectedRowIndex: Number,
            selectedCellIndex: Number
        },
        computed: {
            rowIndex: function () {
                if (!this.componentLoaded)
                    return null;

                return this.$el.parentElement.attributes[0].ownerElement.rowIndex;
            },
            cellIndex: function () {
                if (!this.componentLoaded)
                    return null;

                return this.$el.attributes[0].ownerElement.cellIndex;
            },
            cellRawValue: function () {
                // if (!this.componentLoaded)
                // return null;

                var sumOfValues = 0;

                for (const value of this.values) {
                    if (value["account"] == this.account && value["period"] == this.period) {
                        sumOfValues = sumOfValues + this.textToValue(value["value"]);
                    }
                }

                return sumOfValues;
            },
            cellValue: function () {
                var value = this.cellRawValue;

                if (isNaN(value) || value === "" || value === "-") {
                    return value;
                }

                if (this.invertSign) {
                    value = value * -1;
                }

                var numberFormat = "0,0.00";
                return numeral(value).format(numberFormat);
            },
            isActive: function () {
                if (!this.componentLoaded)
                    return null;

                var rowIndex = this.$el.parentElement.attributes[0].ownerElement.rowIndex;
                var cellIndex = this.$el.attributes[0].ownerElement.cellIndex;

                if (this.rowIndex == this.selectedRowIndex && this.cellIndex == this.selectedCellIndex) {
                    return "active";
                } else {
                    return "";
                }
            }
        },
        watch: {
            // whenever question changes, this function will run
            isActive: function () {
                this.emitSelectedValue();
            }
        },
        mounted: function () {
            this.componentLoaded = true;
        },
        methods: {
            // setIsActive() {
            //     var rowIndex = this.$el.parentElement.attributes[0].ownerElement.rowIndex;
            //     var cellIndex = this.$el.attributes[0].ownerElement.cellIndex;

            //     if (this.rowIndex == this.selectedRowIndex && this.cellIndex == this.selectedCellIndex) {
            //         this.isActive = "active";
            //     } else {
            //         return this.isActive = "";
            //     }
            // },
            textToValue(text) {
                var returnValue = Number(text);

                if (isNaN(returnValue) || returnValue === "" || returnValue === "-") {
                    return 0;
                } else {
                    return returnValue;
                }
            },
            // getRawValue(account, period) {
            //     var sumOfValues = 0;

            //     for (const value of this.values) {
            //         if (value["account"] == account && value["period"] == period) {
            //             sumOfValues = sumOfValues + this.textToValue(value["value"]);
            //         }
            //     }

            //     return sumOfValues;
            // },
            // getValue(account, period, invertSign = false) {
            //     var value = this.getRawValue(account, period);

            //     if (isNaN(value) || value === "" || value === "-") {
            //         return value;
            //     }

            //     if (invertSign) {
            //         value = value * -1;
            //     }

            //     var numberFormat = "0,0.00";
            //     return numeral(value).format(numberFormat);
            // },
            // emitToTable(event) {
            // this.$emit('childToParent', 99)
            // },
            onCellClick(event) {
                // console.log(event.target.cellIndex);
                // console.log(event.target.parentElement.rowIndex);
                this.$emit('onCellClickParent', event.target);
            },
            emitSelectedValue() {

                if (this.isActive) {
                    // console.log('emitSelectedValue');
                    // console.log(this.cellValue);
                    this.$emit('cellSelectedValue', this.cellRawValue);
                }
            }
        }
    };
</script>
<style scoped>
    .cell {
        text-align: right;
    }

    .active {
        background-color: #BDEDFF;
    }
</style>
<template>
    <td class="cell" v-bind:class="[isActive]" v-on:click="onCellClick" v-text="cellValue"></td>
</template>