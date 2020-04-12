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
            account: String,
            period: String,
            invertSign: Boolean,
            values: Array,
            selectedRowIndex: Number,
            selectedCellIndex: Number,
            isEditing: Boolean
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
    .cell {
        text-align: right;
    }

    .active {
        background-color: #BDEDFF;
    }

    .editing {
        background-color: lightskyblue;
    }
</style>
<template>
    <td class="cell" v-bind:class="[isActive, isEditingCss]" v-on:click="onCellClick" v-text="cellValue"></td>
</template>