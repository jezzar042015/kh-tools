<template>
    <div id="timer-display" :class="[
        { 'warning': props.alert.warning },
        { 'overtime': props.alert.overtime }
    ]">
        <div class="digits-holder">
            <div class="digits">{{ tensDigitOfHours }}</div>
            <div class="digits">{{ onesDigitOfHours }}</div>
            <div class="colons">:</div>
            <div class="digits">{{ tensDigitOfMinutes }}</div>
            <div class="digits">{{ onesDigitOfMinutes }}</div>
            <div class="colons">:</div>
            <div class="digits">{{ tensDigitOfSeconds }}</div>
            <div class="digits">{{ onesDigitOfSeconds }}</div>
        </div>
        <TimerControls></TimerControls>
    </div>
</template>

<script setup>
    import { computed } from 'vue';
    import TimerControls from './TimerControls.vue';

    const props = defineProps({
        seconds: Number,
        alert: Object
    });

    const tensDigitOfHours = computed(() => {
        const hours = Math.floor(Math.round(props.seconds) / 3600);
        const tensDigit = Math.floor(hours / 10);
        return tensDigit;
    })

    const onesDigitOfHours = computed(() => {
        const hours = Math.floor(Math.round(props.seconds) / 3600);
        const onesDigit = hours % 10;
        return onesDigit;
    })

    const tensDigitOfMinutes = computed(() => {
        const minutes = Math.floor((Math.round(props.seconds) % 3600) / 60);
        const tensDigit = Math.floor(minutes / 10);
        return tensDigit;
    })

    const onesDigitOfMinutes = computed(() => {
        const minutes = Math.floor((Math.round(props.seconds) % 3600) / 60);
        const onesDigit = minutes % 10;
        return onesDigit;
    })

    const tensDigitOfSeconds = computed(() => {
        const seconds = Math.round(props.seconds) % 60;
        const tensDigit = Math.floor(seconds / 10);
        return tensDigit;
    })

    const onesDigitOfSeconds = computed(() => {
        const seconds = Math.round(props.seconds) % 60;
        const onesDigit = seconds % 10;
        return onesDigit;
    })

</script>
