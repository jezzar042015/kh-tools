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

<script setup>
    import { computed, inject } from 'vue';

    const view = inject('view');
    const live = inject('live');
    const start = inject('start');
    const stop = inject('stop');
    const restart = inject('restart');
    const fwd = inject('fwd',);
    const bwd = inject('bwd');

    const setRestart = computed(() => {
        return view.controls && live.seconds > 0 && !live.running;
    })
    const setStart = computed(() => {
        return view.controls && !live.running;
    })

    const setStop = computed(() => {
        return view.controls && live.running;
    })
    const setJumpers = computed(() => {
        return view.controls && live.running;
    })

</script>