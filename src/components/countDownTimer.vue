<template>
    <span class="timer" :style="{'border': borderStyle}">倒數時間：{{ time }} 秒</span>
</template>

<script>

export default {
    name: 'countDownTimer',
    data() {
        return {
            borderStyle: 'Green',
            time: 70,
            t: 0,
        }
    },
    beforeMount() {
        this.countDown()
    },
    methods: {
        countDown() {
            if( this.time > 0 ) {
                this.t = window.setTimeout(() => {
                    this.time--
                    this.countDown()
                }, 1000)
            }
        },
    },
    watch: {
        time(val) {
            if( val === 0 ){
                this.borderStyle = 'solid 3px Gray'
                console.log(val,"Gray")
                this.$emit('timeUp', "停售")
            }
            else if( 0 < val && val < 60 ) {
                this.borderStyle = 'solid 3px Orange'
                console.log(val,"Orange")
            }
            else if( val >= 60 ) {
                this.borderStyle = 'solid 3px Green'
                console.log(val,"Green")
            }
        }
    },
    beforeDestroy() {
        window.clearTimeout(this.t)
    }
}
</script>