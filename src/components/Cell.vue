<script>
    // https://dev-notes.eu/2018/05/passing-data-between-vue-components/
    // https://laracasts.com/discuss/channels/vue/computed-function-is-running-before-data-is-loaded?page=0

    var numeral = require("numeral");
    export default {
        name: "cell",
        data() {
            return {
                componentLoaded: false
            }
        },
        props: {
            account: Object,
            period: Object,
            invertSign: Boolean,
            values: Array,
            selectedRowIndex: Number,
            selectedCellIndex: Number,
            isEditing: Boolean
        },
        computed: {
            summaryCss: function () {
                if (["quarter", "half", "year"].includes(this.period["type"])) {
                    return "summary";
                }

                return "";
            },
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
            getQuarterPeriods: function () {
                var q_periods;
                var q1_periods = ["1", "2", "3"];
                var q2_periods = ["4", "5", "6"];
                var q3_periods = ["7", "8", "9"];
                var q4_periods = ["10", "11", "12"];

                switch (this.period["id"]) {
                    case "q1":
                        q_periods = q1_periods;
                        break;
                    case "q2":
                        q_periods = q2_periods;
                        break;
                    case "q3":
                        q_periods = q3_periods;
                        break;
                    case "q4":
                        q_periods = q4_periods;
                        break;
                    default:
                        break;
                }

                return q_periods;

            },
            cellRawValue: function () {
                var sumOfValues = 0;
                var periodType = this.period['type'];
                var q_periods = this.getQuarterPeriods;
                switch (periodType) {
                    case "quarter":
                        for (const value of this.values) {
                            if (value["account_id"] == this.account["account"] && q_periods.includes(value["period_id"])) {
                                sumOfValues = sumOfValues + this.textToValue(value["value"]);
                            }
                        }
                        break;

                    case "half":
                        sumOfValues = 2;
                        break;

                    case "year":
                        sumOfValues = 3;
                        break;

                    default:
                        for (const value of this.values) {
                            if (value["account_id"] == this.account["account"] && value["period_id"] == this.period["id"]) {
                                sumOfValues = sumOfValues + this.textToValue(value["value"]);
                            }
                        }
                        break;
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
                var numberFormat = "0,0";
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
            },
            isEditingCss: function () {
                if (this.isActive && this.isEditing) {
                    return "editing";
                } else {
                    return "";
                }
            }
        },
        watch: {
            isActive: function () {
                this.emitSelectedValue();
            }
        },
        mounted: function () {
            this.componentLoaded = true;
        },
        methods: {
            textToValue(text) {
                var returnValue = Number(text);

                if (isNaN(returnValue) || returnValue === "" || returnValue === "-") {
                    return 0;
                } else {
                    return returnValue;
                }
            },
            onCellClick(event) {
                this.$emit('onCellClickParent', event.target);
            },
            emitSelectedValue() {

                if (this.isActive) {
                    this.$emit('cellSelectedValue', this.cellRawValue);
                }
            }
        }
    };
</script>
<style scoped>
    .summary {
        /* background-color: #F6F6F6; */
        color: grey;
    }

    .cell {
        text-align: right;
        padding-right: 3px;
    }

    .active {
        background-color: #BDEDFF;
    }

    .editing {
        background-color: lightskyblue;
    }
</style>
<template>
    <td class="cell" v-bind:class="[isActive, isEditingCss, summaryCss]" v-on:click="onCellClick" v-text="cellValue">
    </td>
</template>