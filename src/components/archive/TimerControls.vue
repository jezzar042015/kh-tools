<template>
    <div class="controls">
        <button class="ctrl" id="backward" v-show="setJumpers">
            <img src="../assets/svg/backward-5.svg" alt="" @click.stop="bwd">
        </button>

        <button class="ctrl" id="start" v-show="setStart" @click.stop="start">
            <img src="../assets/svg/start.svg" alt="">
        </button>

        <button class="ctrl" id="stop" v-show="setStop" @click.stop="stop">
            <img src="../assets/svg/pause.svg" alt="">
        </button>

        <button class="ctrl" id="restart" v-show="setRestart" @click.stop="restart">
            <img src="../assets/svg/replay.svg" alt="">
        </button>

        <button class="ctrl" id="forward" v-show="setJumpers" @click.stop="fwd">
            <img src="../assets/svg/forward-30.svg" alt="">
        </button>
    </div>
</template>

<script>
    export default {
        computed: {
            setRestart() {
                return this.view.controls && this.live.seconds > 0 && !this.live.running
            },
            setStart() {
                return this.view.controls && !this.live.running
            },
            setStop() {
                return this.view.controls && this.live.running
            },
            setJumpers() {
                return this.view.controls && this.live.running
            }
        },
        inject: [
            'view', 'live', 'start', 'stop', 'restart', 'fwd', 'bwd'
        ],
    }
</script>