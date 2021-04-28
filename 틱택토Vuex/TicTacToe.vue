<template>
    <div>
        <div>{{turn}}님의 턴</div>
        <table-component>
            <tr v-for="(rowData, rowIndex) in tableData" :key="rowIndex">
                <td @click="onClickTd(rowIndex, cellIndex)" v-for="(cellData, cellIndex) in rowData" :key="cellIndex">{{cellData}}</td>
            </tr>
        </table-component>
        <div v-if="winner">{{winner}} 님의 승리</div>
    </div>
    
</template>

<script>
import {mapState} from 'vuex'
import TableComponent from './TableComponent.vue';
import store, { CHANGE_TURN, CLICK_CELL, NO_WINNER, RESET_GAME, SET_WINNER } from './store'

export default {
    components: { TableComponent },
    store,
    computed: {
        ...mapState(['tableData', 'winner', 'turn']),
        // winner(){
        //     return this.$store.state.winner
        // },
        // turn(){
        //     return this.$store.state.turn
        // }
    },
    methods: {
        onClickTd(rowIndex, cellIndex){
            if(this.cellData) return;

            this.$store.commit(CLICK_CELL, {row: rowIndex, cell: cellIndex})

            const rootData = this.$root.$data
            console.log(this)
            this.$set(this.tableData[rowIndex], cellIndex, this.turn)

            let win = false
            if(this.tableData[rowIndex][0] === this.turn && this.tableData[rowIndex][1] === this.turn && this.tableData[rowIndex][2] === this.turn){
                win = true
            }
            if(this.tableData[0][cellIndex] === this.turn && this.tableData[1][cellIndex] === this.turn && this.tableData[2][cellIndex] === this.turn){
                win = true
            }
            if(this.tableData[0][0] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][2] === this.turn){
                win = true
            }
            if(this.tableData[0][2] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][0] === this.turn){
                win = true
            }

            if(win){
                this.$store.commit(SET_WINNER, this.turn)
                this.$store.commit(RESET_GAME)
            }else { // 지거나 무승부
                let all = true
                this.tableData.forEach(row => {
                    row.forEach(cell => {
                        if(!cell){
                            all = false
                        }
                    })
                });

                if(all){
                    this.$store.commit(NO_WINNER)
                    this.$store.commit(RESET_GAME)
                }else{
                    this.$store.commit(CHANGE_TURN)
                }
            }
        }
    }
}
</script>

<style>
table{
    border-collapse: collapse;
}
td{
    border: 1px solid black;
    width: 40px;
    height: 40px;
    text-align: center;
}
</style>