<template>
    <div :class="[part.class ?? 'part-item', { active: part.active }]" @click="activatePart">
        <!-- icon -->
        <div class="icon" v-if="part.icon">
            <img :src="getImageUrl(part.icon)" alt="">
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
        },
        getImageUrl(name) {
            return new URL(`../assets/svg/${name}`, import.meta.url).href
        }
    },
    inject: [
        'setTimerSeconds', 'selectPart'
    ]
}
</script>