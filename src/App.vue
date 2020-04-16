<!-- https://dev-notes.eu/2018/05/passing-data-between-vue-components/ -->
<!-- https://medium.com/js-dojo/vue-js-single-file-components-vue-cli-and-example-of-how-to-build-reusable-components-cf0991adbc2f -->
<template>
    <div id="app">
        <h1>My Vue App</h1>
        <!-- PersonGreeter personName="Travis" / -->
        <div class="input-group">
            <input v-model="selectedCellValueOrKeyedValue" type="text" class="form-control"
                aria-label="Recipient's username" aria-describedby="basic-addon2">
            <div class="input-group-append">
                <button class="btn btn-outline-secondary" type="button">OK</button>
                <button class="btn btn-outline-secondary" type="button">Esc</button>
                <button class="btn btn-outline-secondary" type="button">fx()</button>
            </div>
        </div>
        <Table v-on:tableEditing="tableEditing" v-on:cellSelectedValue="cellSelectedValue" :accounts="accounts"
            :periods="periods" :values="values" :isEditing="isEditing" :keyedValue="keyedValue"
            :selectedCellValue="selectedCellValue" v-on:tableKeyedValue="tableKeyedValue" />
    </div>
</template>
<script>
    // import PersonGreeter from "./components/PersonGreeter.vue";
    import Table from "./components/Table.vue";
    export default {
        name: "app",
        data() {
            return {
                keyedValue: "",
                isEditing: false,
                selectedCellValue: null,
                accounts: [
                    {
                        "account": "4000",
                        "name": "4000 - Main Sales",
                        "isLeaf": true,
                        "invert": true
                    },
                    {
                        "account": "4100",
                        "name": "4100 - Other Sales",
                        "isLeaf": true,
                        "invert": true
                    },
                    {
                        "account": "tot_sales",
                        "name": "Total Sales",
                        "isLeaf": false,
                        "invert": true
                    },
                    {
                        "account": "5000",
                        "name": "5000 - Cost of Goods",
                        "isLeaf": true,
                        "invert": false
                    },
                    {
                        "account": "tot_cos",
                        "name": "Total COS",
                        "isLeaf": false,
                        "invert": true
                    },
                    {
                        "account": "gm",
                        "name": "Gross Margin",
                        "isLeaf": false,
                        "invert": true
                    },
                    {
                        "account": "6000",
                        "name": "6000 - Expenses",
                        "isLeaf": true,
                        "invert": false
                    },
                    {
                        "account": "tot_exp",
                        "name": "Total Expenses",
                        "isLeaf": false,
                        "invert": true
                    },
                    {
                        "account": "np",
                        "name": "Net Profit",
                        "isLeaf": false,
                        "invert": true
                    }
                ],
                periods: [
                    {
                        "id": "1",
                        "name": "Jul-19",
                        "type": "month"
                    },
                    {
                        "id": "2",
                        "name": "Aug-19",
                        "type": "month"
                    },
                    {
                        "id": "3",
                        "name": "Sep-19",
                        "type": "month"
                    },
                    {
                        "id": "q1",
                        "name": "Q1-20",
                        "type": "quarter"
                    },
                    {
                        "id": "4",
                        "name": "Oct-19",
                        "type": "month"
                    },
                    {
                        "id": "5",
                        "name": "Nov-19",
                        "type": "month"
                    },
                    {
                        "id": "6",
                        "name": "Dec-19",
                        "type": "month"
                    },
                    {
                        "id": "q2",
                        "name": "Q2-20",
                        "type": "quarter"
                    },
                    {
                        "id": "h1",
                        "name": "H1-20",
                        "type": "half"
                    },
                    {
                        "id": "7",
                        "name": "Jan-20",
                        "type": "month"
                    },
                    {
                        "id": "8",
                        "name": "Feb-20",
                        "type": "month"
                    },
                    {
                        "id": "9",
                        "name": "Mar-20",
                        "type": "month"
                    },
                    {
                        "id": "q3",
                        "name": "Q3-20",
                        "type": "quarter"
                    },
                    {
                        "id": "10",
                        "name": "Apr-20",
                        "type": "month"
                    },
                    {
                        "id": "11",
                        "name": "May-20",
                        "type": "month"
                    },
                    {
                        "id": "12",
                        "name": "Jun-20",
                        "type": "month"
                    },
                    {
                        "id": "q4",
                        "name": "Q4-20",
                        "type": "quarter"
                    },
                    {
                        "id": "h2",
                        "name": "H2-20",
                        "type": "half"
                    },
                    {
                        "id": "yr",
                        "name": "2020",
                        "type": "year"
                    }
                ],
                values: [
                    {
                        "account_id": "4000",
                        "period_id": "1",
                        "quarter_id": "q1",
                        "half_id": "h1",
                        "year_id": "2020",
                        "value": "1111.11"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "2",
                        "quarter_id": "q1",
                        "half_id": "h1",
                        "year_id": "2020",
                        "value": "2222.22"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "3",
                        "quarter_id": "q1",
                        "half_id": "h1",
                        "year_id": "2020",
                        "value": "3333.33"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "4",
                        "quarter_id": "q2",
                        "half_id": "h1",
                        "year_id": "2020",
                        "value": "4444.44"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "5",
                        "quarter_id": "q2",
                        "half_id": "h1",
                        "year_id": "2020",
                        "value": "5555.55"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "6",
                        "quarter_id": "q2",
                        "half_id": "h1",
                        "year_id": "2020",
                        "value": "6666.66"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "7",
                        "quarter_id": "q3",
                        "half_id": "h2",
                        "year_id": "2020",
                        "value": "7777.77"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "8",
                        "quarter_id": "q3",
                        "half_id": "h2",
                        "year_id": "2020",
                        "value": "8888.88"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "9",
                        "quarter_id": "q3",
                        "half_id": "h2",
                        "year_id": "2020",
                        "value": "9999.99"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "10",
                        "quarter_id": "q4",
                        "half_id": "h2",
                        "year_id": "2020",
                        "value": "1010.00"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "11",
                        "quarter_id": "q4",
                        "half_id": "h2",
                        "year_id": "2020",
                        "value": "1111.00"
                    },
                    {
                        "account_id": "4000",
                        "period_id": "12",
                        "quarter_id": "q4",
                        "half_id": "h2",
                        "year_id": "2020",
                        "value": "1212.00"
                    }
                ]
            }
        },
        methods: {
            tableKeyedValue(keyedValue) {
                this.keyedValue = keyedValue;
            },
            tableEditing(isEditing) {
                this.isEditing = isEditing;
            },
            cellSelectedValue(activeCellValue) {
                this.selectedCellValue = activeCellValue;
            }
        },
        components: {
            // PersonGreeter,
            Table
        },
        computed: {
            selectedCellValueOrKeyedValue: function () {
                if (this.isEditing) {
                    return this.keyedValue;
                } else {
                    return this.selectedCellValue;
                }
            }
        },
        mounted() {
            const self = this;
            // NOTE: Something about this code makes scrolling work!
            // var check_width = window.innerWidth;
            // console.log(check_width);
            // window.addEventListener("resize", function () {
            //     var check_width = window.innerWidth;
            //     console.log(check_width);
            // });
            document.querySelector("table").click();
        }
    };
</script>

<style>
    #app {
        font-family: Helvetica, Arial, sans-serif;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    h1 {
        margin-bottom: 40px;
    }

    .input-group {
        margin-bottom: 20px;
    }
</style>