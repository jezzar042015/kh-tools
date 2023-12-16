<template>
    <div id="timer-display" :class="[
        { 'warning': alert.warning },
        { 'overtime': alert.overtime }
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

<script>
import TimerControls from './TimerControls.vue';

export default {
    name: 'TimerDisplay',
    components: {
        TimerControls
    },
    props: {
        seconds: Number,
        alert: Object
    },
    data() {
        return {

        }
    },
    computed: {
        tensDigitOfHours() {
            const hours = Math.floor(Math.round(this.seconds) / 3600);
            const tensDigit = Math.floor(hours / 10);
            return tensDigit;
        },
        onesDigitOfHours() {
            const hours = Math.floor(Math.round(this.seconds) / 3600);
            const onesDigit = hours % 10;
            return onesDigit;
        },
        tensDigitOfMinutes() {
            const minutes = Math.floor((Math.round(this.seconds) % 3600) / 60);
            const tensDigit = Math.floor(minutes / 10);
            return tensDigit;
        },
        onesDigitOfMinutes() {
            const minutes = Math.floor((Math.round(this.seconds) % 3600) / 60);
            const onesDigit = minutes % 10;
            return onesDigit;
        },
        tensDigitOfSeconds() {
            const seconds = Math.round(this.seconds) % 60;
            const tensDigit = Math.floor(seconds / 10);
            return tensDigit;
        },
        onesDigitOfSeconds() {
            const seconds = Math.round(this.seconds) % 60;
            const onesDigit = seconds % 10;
            return onesDigit;
        },
    }
}
</script>

<style>
#timer-display
{
    width: 100%;
    height: 100%;
    display: flex;
    flex-flow: column;
    justify-content: center;
    align-items: center;
    user-select: none;
    gap: 20px;
}
</style>

<style scoped>
.digits-holder
{
    width: 100%;
    height: fit-content;
    display: flex;
    flex-flow: row;
    justify-content: center;
}

.digits
{
    color: white;
    font-weight: 700;
    font-size: 15vw;
    width: 11vw;
    display: flex;
    justify-content: center;
}

.colons
{
    font-weight: 700;
    font-size: 8vw;
    width: 90px;
    display: flex;
    justify-content: center;
    color: white;
    align-items: center;
}

/* on phones */
@media screen and (max-width: 767px)
{
    .digits-holder
    {
        position: absolute;
        top: 180px;
    }

    .colons
    {
        width: 30px;
        font-size: 4vh;
    }

    .digits
    {
        font-weight: 700;
        font-size: 17vw;
        width: 12vw;
    }
}
</style>

<style scoped>
.warning
{
    background: orange;
}

.overtime
{
    animation: blinkingBackground 1s infinite;
}

@keyframes blinkingBackground
{

    50%
    {
        background-color: #ef0a1a;
    }

}
</style>
