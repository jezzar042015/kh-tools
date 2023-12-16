<template>
    <div :class="[part.class ?? 'part-item', { active: part.active }]" @click="activatePart">
        <!-- icon -->
        <div class="icon" v-if="part.icon">
            <img :src="`./src/assets/svg/${part.icon}`" alt="">
            <!-- <img src="../assets/svg/living.svg" alt=""> -->
            <!-- {{ `@/assets/svg/${part.icon}` }} -->
        </div>
        <!-- label -->
        <div class="details">
            <div class="label">
                <div class="partname">{{ part.part }}</div>
                <div>{{ getDuration }}</div>
            </div>
            <div class="label">
                <div class="participant">
                    {{ part.participant }}
                </div>
                <div class="remark" v-if="part.timed">
                    <div :class="['remark-icon', setRemarksType]">
                        <div>
                            {{ displayRemarksTs }}
                        </div>
                    </div>

                </div>
            </div>


        </div>
    </div>
</template>

<script>
export default {
    props: {
        part: Object
    },
    computed: {
        getDuration() {
            return this.part.limit ? this.part.limit + 'm' : ''
        },
        setRemarksType() {
            if (!this.part.consumed || this.part.consumed == 0) {
                return null
            } else {
                return (this.part.consumed > (this.part.limit * 60)) ? 'over' : 'under'
            }
        },
        displayRemarksTs() {
            if (this.part.consumed && this.part.consumed > 0) {
                let diff = Math.abs(this.part.consumed - (this.part.limit * 60))

                const minutes = Math.floor(diff / 60);
                const remainingSeconds = Math.round(diff % 60);

                const minutesString = minutes > 0 ? `${minutes}m` : '';
                const secondsString = `${remainingSeconds}s`;

                return `${minutesString} ${secondsString}`;
            } else {
                return null
            }
        }
    },
    methods: {
        activatePart() {
            if (this.part.timed) {
                this.setTimerSeconds(this.part.consumed)
                this.selectPart(this.part.id);
            }
        }
    },
    inject: [
        'setTimerSeconds', 'selectPart'
    ]
}
</script>

<style scoped>
.icon
{
    height: 100%;
    width: 30px;
    display: flex;
    align-items: center;
}

.details
{
    width: 100%;
    display: flex;
    flex-flow: column;
    gap: 3px;
}

.label
{
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.partname
{
    font-weight: 500;
}

.participant
{
    font-size: 12px;
}

.part-item
{
    padding: 20px 15px;
    background: transparent;
    width: 100%;
    height: fit-content;
    color: white;
    font-weight: 300;
    font-size: 14px;

    display: flex;
    align-items: center;
    cursor: pointer;
}

.active
{
    background: #ffffff1e;
}

.part-item:hover
{
    background: #ffffff1e;
}

.mwb-treasures,
.mwb-ministry,
.mwb-living
{
    padding: 5px 15px;
    width: 100%;
    min-height: 50px;
    color: white;
    font-weight: 500;
    font-size: 14px;

    display: flex;
    align-items: center;
    gap: 5px;
}

.mwb-treasures
{
    background: #808080;
}

.mwb-ministry
{
    background: #ffa500;
}

.mwb-living
{
    background: #99131E;
}

.remark
{
    display: flex;
    align-items: center;
    padding: 3px 0;
    gap: 4px;
    font-size: 12px;
}

.remark-icon
{
    padding: 2px 10px;
    height: fit-content;
    width: fit-content;
    border-radius: 10px;
}

.over
{
    background: #ec4754;
}

.under
{
    background: rgb(0, 190, 0);
    color: black;
}

@media screen and (max-width: 767px)
{
    .details
    {
        user-select: none;
    }
}
</style>