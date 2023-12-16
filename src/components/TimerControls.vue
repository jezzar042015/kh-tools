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

<style scoped>
.controls
{
    height: 100px;
    width: 100%;
    padding: 30px;
    display: flex;
    justify-content: center;
    gap: 30px;
}

.ctrl
{
    height: 85px;
    width: 85px;
    border-radius: 50%;
    border: none;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    background: #d3d3d317;
}

.ctrl:hover
{
    background: #ffffff4d;

}

#start img
{
    height: 85%;
    background-color: transparent;
}

#stop img
{
    height: 65%;
}

#restart img
{
    height: 65%;
}

#forward img
{
    height: 75%;
}

#backward img
{
    height: 75%;
}

/* on phones */
@media screen and (max-width: 767px)
{
    .controls
    {
        position: absolute;
        top: 95px;
        height: auto;
        padding: 5px 25px;

        justify-content: center;
    }

    .ctrl
    {
        height: 55px;
        width: 55px;
        background: white;
    }
}</style>