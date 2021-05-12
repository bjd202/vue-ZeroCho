<template>
    <div v-if="curruntGame === 'number-baseball'">
        <number-baseball></number-baseball>
    </div>
    <div v-else-if="curruntGame === 'response-check'">
        <response-check></response-check>
    </div>
    <div v-else-if="curruntGame === 'rock-se'">
        <rock></rock>
    </div>
    <div v-else-if="curruntGame === 'lotto-generator'">
        <lotto-generator></lotto-generator>
    </div>
    <div v-else>
        일치하는 게임이 없습니다.
    </div>
</template>

<script>
import NumberBaseball from '../숫자야구/NumberBaseball'
import ResponseCheck from '../반응속도/ResponseCheck'
import RockSe from '../가위바위보/Rock'
import LottoGenerator from '../로또/LottoGenerator.vue'
export default {
    components: { 
        NumberBaseball,
        ResponseCheck,
        RockSe,
        LottoGenerator,
    },
    mounted(){
        console.log(this.$router)
        console.log(this.$route)
    },
    computed: {
        curruntGame(){
            return this.$route.params.name;
        }
    }
}
</script>
