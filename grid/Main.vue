<template>
    <div>
        <form id="search">
            search <input name="query" v-model="searchQuery">
        </form>
        <GridComponent
         :gridData="gridData"
         :columns="gridColumns"
         :filter-key="searchQuery"
        ></GridComponent>
    </div>
</template>

<script>
    import GridComponent from './GridComponent.vue'
    export default {
        components: {
            GridComponent
        },
        data() {
            return {
                searchQuery: '',
                gridColumns: ['name', 'power'],
                gridData: [
                    { name: 'Chuck Norris', power: Infinity },
                    { name: 'Bruce Lee', power: 9000 },
                    { name: 'Jackie Chan', power: 7000 },
                    { name: 'Jet Li', power: 8000 }
                ]
            }
        }
    }
</script>

<style>
    
</style>
