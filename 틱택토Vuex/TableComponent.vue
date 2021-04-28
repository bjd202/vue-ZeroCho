<template>
    <table>
        <!-- <tr-component v-for="(rowData, index) in tableData" :key="index" :row-data="rowData" :row-index="index"></tr-component> -->
        <slot />
    </table>
    
</template>

<script>
import {mapState} from 'vuex'
import TrComponent from './TrComponent'

export default {
    props: {
        tableData: Array,
    },
    components: {
        TrComponent
    },
    data(){
        return {
            
        }
    },
    computed:{
        ...mapState(['tableData']),
        tableData() {
            return this.$store.state.tableData
        },
        turnMessage(){
            return this.$store.getters.turnMessage
        }
    },
    methods:{
       
    },
}
</script>

<style>

</style>