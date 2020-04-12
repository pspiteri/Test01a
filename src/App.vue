<!-- https://dev-notes.eu/2018/05/passing-data-between-vue-components/ -->
<!-- https://medium.com/js-dojo/vue-js-single-file-components-vue-cli-and-example-of-how-to-build-reusable-components-cf0991adbc2f -->
<template>
    <div id="app">
        <h1>My Vue App</h1>
        <!-- PersonGreeter personName="Travis" / -->
        <Table :accounts="accounts" :periods="periods" :values="values" />
    </div>
</template>
<script>
    // import PersonGreeter from "./components/PersonGreeter.vue";
    import Table from "./components/Table.vue";
    export default {
        name: "app",
        data() {
            return {
                // selectedValue: "",
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
                ]
            }
        },
        methods: {

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
</style>