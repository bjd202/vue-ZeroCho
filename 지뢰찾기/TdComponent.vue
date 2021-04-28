<template>
    <td @click="onClickTd">{{cellData}}</td>
</template>

<script>
import {mapState} from 'vuex'
import {CLICK_CELL, NO_WINNER} from './store'
import {SET_WINNER} from './store'
import {RESET_GAME} from './store'
import {CHANGE_TURN} from './store'

export default {
    props: {
        cellData: String,
        rowIndex: Number,
        cellIndex: Number
    },
    computed: {
        ...mapState({
            tableData: state => state.tableData,
            turn: state => state.turn,
            cellData(state){
                return state.tableData[this.rowIndex][this.cellData]
            }
        }),
        // cellData() {
        //     return this.$store.state.tableData[this.rowIndex][this.cellIndex]
        // },
        // tableData() {
        //     return this.$store.state.tableData
        // },
        // turn() {
        //     return this.$store.state.turn
        // }
    },
    methods: {
        onClickTd(){
            if(this.cellData) return;

            this.$store.commit(CLICK_CELL, {row: this.rowIndex, cell: this.cellIndex})

            const rootData = this.$root.$data
            console.log(this)
            this.$set(this.tableData[this.rowIndex], this.cellIndex, this.turn)

            let win = false
            if(this.tableData[this.rowIndex][0] === this.turn && this.tableData[this.rowIndex][1] === this.turn && this.tableData[this.rowIndex][2] === this.turn){
                win = true
            }
            if(this.tableData[0][this.cellIndex] === this.turn && this.tableData[1][this.cellIndex] === this.turn && this.tableData[2][this.cellIndex] === this.turn){
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