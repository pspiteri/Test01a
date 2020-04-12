<script>
    // import Cell from "./Cell.vue";
    import Row from "./Row.vue";
    export default {
        name: "entry-table",
        components: {
            // Cell
            Row
        },
        props: {
            accounts: Array,
            periods: Array,
            values: Array
        },
        data() {
            return {
                selectedRowIndex: 1,
                selectedCellIndex: 1
            }
        },
        computed: {
            columnCount: function () {
                return this.periods.length;
            },
            rowCount: function () {
                return this.accounts.length;
            }
        },
        methods: {
            onCellClickParent(target) {
                // console.log('onCellClickPARENT');
                // console.log(target);
                this.selectedCellIndex = target.cellIndex;
                this.selectedRowIndex = target.parentElement.rowIndex;
            },
            navigation(key) {
                console.log('navigation');

                switch (key) {
                    case "ArrowLeft":
                        if (this.selectedCellIndex > 1) {
                            this.selectedCellIndex--;
                        }

                        break;
                    case "ArrowRight":
                        if (this.selectedCellIndex < this.columnCount) {
                            this.selectedCellIndex++;
                        }

                        break;
                    case "ArrowUp":
                        if (this.selectedRowIndex > 1) {
                            this.selectedRowIndex--;
                        }
                        break;
                    case "ArrowDown":
                        if (this.selectedRowIndex < this.rowCount) {
                            this.selectedRowIndex++;
                        }

                        break;
                    default:
                        break;
                }
            }
        },
        mounted() {
            const self = this;
            window.addEventListener("keydown", function (e) {
                // use self instead of this in here
                console.log(e.key);
                self.navigation(e.key);
            });
        }
    };
</script>
<style scoped>
    .row-header {
        text-align: left;
    }

    .col-header {
        text-align: right;
    }
</style>
<template>
    <div class="table-responsive-md">
        <table class="table table-sm" id="table">
            <thead>
                <tr>
                    <th class="row-header" scope="col" width="150px"></th>
                    <th class="col-header" v-for="period in periods" scope="col" width="75px">{{ period["name"] }}
                    </th>
                </tr>
            </thead>
            <tbody>
                <template v-for="account in accounts">
                    <Row v-on:onCellClickParent="onCellClickParent" :account="account['name']" :periods="periods"
                        :values="values" :selectedRowIndex="selectedRowIndex" :selectedCellIndex="selectedCellIndex" />
                </template>
            </tbody>
        </table>
    </div>
</template>