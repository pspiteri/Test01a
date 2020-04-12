<script>
    import Row from "./Row.vue";
    export default {
        name: "entry-table",
        components: {
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
                selectedCellIndex: 1,
                editing: false,
                keyedValue: ""
            }
        },
        computed: {
            columnCount: function () {
                return this.periods.length;
            },
            rowCount: function () {
                return this.accounts.length;
            },
        },
        methods: {
            onCellClickParent(target) {
                this.selectedCellIndex = target.cellIndex;
                this.selectedRowIndex = target.parentElement.rowIndex;
            },
            navigation(key) {
                switch (key) {
                    case "Alt":
                        break;
                    case "Escape":
                        this.editing = false;
                        this.keyedValue = "";
                        break;
                    case "ArrowLeft":
                        if (this.selectedCellIndex > 1) {
                            this.selectedCellIndex--;
                        }
                        this.editing = false;
                        this.keyedValue = "";
                        break;
                    case "ArrowRight":
                        if (this.selectedCellIndex < this.columnCount) {
                            this.selectedCellIndex++;
                        }
                        this.editing = false;
                        this.keyedValue = "";
                        break;
                    case "ArrowUp":
                        if (this.selectedRowIndex > 1) {
                            this.selectedRowIndex--;
                        }
                        this.editing = false;
                        this.keyedValue = "";
                        break;
                    case "ArrowDown":
                        if (this.selectedRowIndex < this.rowCount) {
                            this.selectedRowIndex++;
                        }
                        this.editing = false;
                        this.keyedValue = "";
                        break;
                    default:
                        if (!this.editing) { this.editing = true }

                        if (this.editing) {
                            this.keyedValue = this.keyedValue + key;
                        } else {
                            return key;
                        }

                        console.log(this.keyedValue);
                        break;
                }
            }
        },
        mounted() {
            const self = this;
            window.addEventListener("keydown", function (e) {
                // use self instead of this in here
                // console.log(e.key);
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
                        :values="values" :selectedRowIndex="selectedRowIndex" :selectedCellIndex="selectedCellIndex"
                        v-on="$listeners" />
                </template>
            </tbody>
        </table>
    </div>
</template>