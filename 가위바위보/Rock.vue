<template>
    <div>
        <div id="screen" v-bind:class="state" @click="onClickScreen">
            {{message}}
        </div>
        <template v-if="result.length">
            <div>평균 시간 : {{average}}ms</div>
            <button @click="onReset">리셋</button>
        </template>
    </div>
</template>

<script>
let startTime = 0
let endTime = 0
let timeout = null
export default {
    data() {
        return {
            result: [],
            state: 'waiting',
            message: '클릭해서 시작'
        }
    },
    computed: {
        average() {
            return this.result.reduce((a, c) => a + c, 0) / this.result.length || 0
        }
    },
    methods: {
        onReset() {
            this.result = []
        },
        onClickScreen() {
            if(this.state === 'waiting'){
                this.state = 'ready'
                this.message = '초록색이 되면 클릭'
                timeout = setTimeout(() => {
                    this.state = 'now'
                    this.message = '지금 클릭!'
                    startTime = new Date()
                }, Math.floor(Math.random()*1000 + 2000));
            }else if(this.state === 'ready'){
                clearTimeout(timeout)
                this.state = 'waiting'
                this.message = '너무 성급! 초록색이 된 후에 클릭'
            }else if(this.state === 'now'){
                endTime = new Date()
                this.state = 'waiting'
                this.message = '클릭해서 시작'
                this.result.push(endTime - startTime)
            }
        }
    }
}
</script>

<style>
#screen {
    width: 300px;
    height: 200px;
    text-align: center;
    user-select: none;
}

#screen.waiting {
    background-color: aqua;
}

#screen.ready {
    background-color: red;
    color: white;
}

#screen.now {
    background-color: greenyellow;
}
</style>