<!-- https://dev-notes.eu/2018/05/passing-data-between-vue-components/ -->
<!-- https://medium.com/js-dojo/vue-js-single-file-components-vue-cli-and-example-of-how-to-build-reusable-components-cf0991adbc2f -->
<template>
    <div id="app">
        <h1>My Vue App</h1>
        <!-- PersonGreeter personName="Travis" / -->
        <div class="input-group">
            <input v-model="selectedCellValue" type="text" class="form-control" aria-label="Recipient's username"
                aria-describedby="basic-addon2">
            <div class="input-group-append">
                <button class="btn btn-outline-secondary" type="button">OK</button>
                <button class="btn btn-outline-secondary" type="button">Esc</button>
                <button class="btn btn-outline-secondary" type="button">fx()</button>
            </div>
        </div>
        <Table v-on:tableKeyedValue="tableKeyedValue" v-on:tableEditing="tableEditing"
            v-on:cellSelectedValue="cellSelectedValue" :accounts="accounts" :periods="periods" :values="values"
            :isEditing="isEditing" :keyedValue="keyedValue" :selectedCellValue="selectedCellValue" />
    </div>
</template>
<script>
    // import PersonGreeter from "./components/PersonGreeter.vue";
    import Table from "./components/Table.vue";
    export default {
        name: "app",
        data() {
            return {
                keyedValue: null,
                isEditing: false,
                selectedCellValue: null,
                accounts: [
                    {
                        "name": "4000",
                        "invert": false
                    },
                    {
                        "name": "5000",
                        "invert": false
                    },
                    {
                        "name": "6000",
                        "invert": false
                    }
                ],
                periods: [
                    {
                        "name": "1"
                    },
                    {
                        "name": "2"
                    },
                    {
                        "name": "3"
                    },
                    {
                        "name": "4"
                    },
                    {
                        "name": "5"
                    },
                    {
                        "name": "6"
                    },
                    {
                        "name": "7"
                    },
                    {
                        "name": "8"
                    },
                    {
                        "name": "9"
                    },
                    {
                        "name": "10"
                    },
                    {
                        "name": "11"
                    },
                    {
                        "name": "12"
                    }
                ],
                values: [
                    {
                        "account": "4000",
                        "period": "1",
                        "value": "1111.11"
                    },
                    {
                        "account": "4000",
                        "period": "2",
                        "value": "2222.22"
                    },
                    {
                        "account": "4000",
                        "period": "3",
                        "value": "3333.33"
                    },
                    {
                        "account": "4000",
                        "period": "4",
                        "value": "4444.44"
                    },
                    {
                        "account": "4000",
                        "period": "5",
                        "value": "5555.55"
                    },
                    {
                        "account": "4000",
                        "period": "6",
                        "value": "6666.66"
                    },
                    {
                        "account": "4000",
                        "period": "7",
                        "value": "7777.77"
                    },
                    {
                        "account": "4000",
                        "period": "8",
                        "value": "8888.88"
                    },
                    {
                        "account": "4000",
                        "period": "9",
                        "value": "9999.99"
                    },
                    {
                        "account": "4000",
                        "period": "10",
                        "value": "1010.00"
                    },
                    {
                        "account": "4000",
                        "period": "11",
                        "value": "1111.00"
                    },
                    {
                        "account": "4000",
                        "period": "12",
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