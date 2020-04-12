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
            values: Array,
            isEditing: Boolean,
            keyedValue: Number,
            selectedCellValue: Number
        },
        data() {
            return {
                selectedRowIndex: 1,
                selectedCellIndex: 1
                // editing: false,
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
            resetNavigationProperties() {
                // this.$emit('tableEditing', false);
                // this.$emit('tableKeyedValue', "");
                this.setEditingToFalse();
                this.resetKeyedValue();
            },
            resetKeyedValue() {
                this.$emit('tableKeyedValue', "");
            },
            setEditingToFalse() {
                this.$emit('tableEditing', false);
            },
            setEditingToTrue() {
                this.$emit('tableEditing', true);
            },
            setKeyedValueToSelectedValue(key) {
                this.$emit('tableKeyedValue', this.selectedCellValue);
            },
            addKeyToKeyedValueIfEditing(key) {
                this.$emit('tableKeyedValue', this.keyedValue + key);
            },
            navigation(key) {
                switch (key) {
                    case "Alt":
                        break;
                    case "Escape":
                        this.resetNavigationProperties();
                        break;
                    case "ArrowLeft":
                        if (this.selectedCellIndex > 1) {
                            this.selectedCellIndex--;
                        }
                        this.setKeyedValueToSelectedValue();
                        this.setEditingToFalse();
                        // this.resetNavigationProperties();
                        break;
                    case "ArrowRight":
                        if (this.selectedCellIndex < this.columnCount) {
                            this.selectedCellIndex++;
                        }

                        this.setKeyedValueToSelectedValue();
                        this.setEditingToFalse();
                        // this.resetNavigationProperties();
                        break;
                    case "ArrowUp":
                        if (this.selectedRowIndex > 1) {
                            this.selectedRowIndex--;
                        }

                        this.setKeyedValueToSelectedValue();
                        this.setEditingToFalse();
                        // this.resetNavigationProperties();
                        break;
                    case "ArrowDown":
                        if (this.selectedRowIndex < this.rowCount) {
                            this.selectedRowIndex++;
                        }

                        this.setKeyedValueToSelectedValue();
                        this.setEditingToFalse();
                        // this.resetNavigationProperties();
                        break;
                    default:
                        if (!this.isEditing) { this.resetKeyedValue(); }
                        this.setEditingToTrue();
                        this.addKeyToKeyedValueIfEditing(key);

                        console.log(this.keyedValue);
                        break;
                }
            }
        },
        mounted() {
            const self = this;
            window.addEventListener("keydown", function (e) {
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
                        :isEditing="isEditing" v-on="$listeners" />
                </template>
            </tbody>
        </table>
    </div>
</template>