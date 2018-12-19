<template>
    <table>
        <thead>
            <tr>
                <th
                 v-for="key in columns"
                 :key="key"
                 @click="sortBy(key)"
                 :class="{ active: sortKey == key }"
                >
                    {{  key | capitalize }}
                    <span
                     class="arrow"
                     :class="sortOrders[key] ? 'asc' : 'dsc'" 
                    ></span>
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="entry in filteredData" :key="entry">
                <td v-for="key in columns" :key="key">
                    {{ entry[key] }}
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    // tbody 中数据一般是 [{},{},{}] 这种格式的，双层循环
    export default {
        props: {
            gridData: Array,
            columns: Array,
            filterKey: String
        },
        data() {
            // 又学一招，再 return 前可以对数据处理再返回
            var sortOrders = {}
            this.columns.foreach(function(col) {
                sortOrders[col] = 1
            })
            return {
                sortKey: '',
                sortOrders: sortOrders
            }
        },
        computed: {
            filteredData() {
                // 先把要用的数据取出来 ，是个好习惯
                var filterKey = this.filterKey && this.filterKey.toLowerCase()
                var sortKey = this.sortKey
                var order = this.sortOrders[sortKey] || 1
                var data = this.gridData
                if(filterKey) {
                    // 双层循环，第一层，过滤每个 row
                    // 第二层，只要 key 中有满足的
                    data = data.filter(function(row) {
                        return Object.keys(row).some(function(key) {
                            return String(row[key]).toLowerCase().indexOf(filterKey) > -1
                        })
                    })
                }
                if(filterKey) {
                    data = data.filter(function(row) {
                        return Object.keys(row).some(function(key) {
                            return row[key].indexOf(filterKey) > -1
                        })
                    })
                }
                if(sortKey) {
                    // 对象排序写的也很帅
                    data = data.slice().sort(function (a, b) {
                        a = a[sortKey]
                        b = b[sortKey]
                        return ( a === b ? 0 : a > b ? 1 : -1 ) * order
                    })
                }
                return data
            }
        },
        filters: {
            capitalize: function (str) {
                return str.charAt(0).toUpperCase() + str.slice(1)
            }
        },
        methods: {
            sortBy(key) {
                this.sortKey = key
                this.sortOrders[key]  = this.sortOrders[key] * -1
            }
        }
    }
</script>

<style>
    body {
        font-family: 'Helvetica Neue', Arial, sans-serif;
        font-size: 14px;
        color: #333;
    }

    table {
        border: 2px solid #42b983;
        border-radius: 3px;
        background: #fff;
    }
    th {
        background-color: #42b983;
        color: rgba(255, 255, 255, 0.66);
        cursor: pointer;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }
    td {
        background-color: #f9f9f9;
    }
    th, td {
        min-width: 120px;/* table 单元格设置，那如果按列设置呢？ */
        padding: 10px 20px;
    }
    th.active {
        color: #fff;
    }
    th.active .arrorw {
        opacity: 1;
    }
    .arrow {
        display: inline-block;
        vertical-align: middle;
        width: 0;
        height: 0;
        margin-left: 5px;
        opacity: 0.66;
    }
</style>
