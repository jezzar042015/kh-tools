<template>
    <div :class="[props.part.class ?? 'part-item', { active: props.part.active }]" @click="activatePart">
        <!-- icon -->
        <div class="icon" v-if="props.part.icon">
            <img :src="getImageUrl(props.part.icon)" alt="">
        </div>
        <!-- label -->
        <div class="details">
            <div class="label">
                <div class="partname">{{ props.part.part }}</div>
                <div>{{ getDuration }}</div>
            </div>
            <div class="label">
                <div class="participant">
                    {{ props.part.participant }}
                </div>
                <div class="remark" v-if="props.part.timed">
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

<script setup>
    import { computed, inject } from 'vue';

    const props = defineProps({
        part: { type: Object, }
    });

    const getDuration = computed(() => {
        return props.part.limit ? props.part.limit + 'm' : '';
    });

    const setRemarksType = computed(() => {
        if (!props.part.consumed || props.part.consumed == 0) {
            return null;
        } else {
            return (props.part.consumed > (props.part.limit * 60))
                ? 'over' : 'under';
        }
    });

    const displayRemarksTs = computed(() => {
        if (props.part.consumed && props.part.consumed > 0) {
            let diff = Math.abs(props.part.consumed - (props.part.limit * 60))

            const minutes = Math.floor(diff / 60);
            const remainingSeconds = Math.round(diff % 60);

            const minutesString = minutes > 0 ? `${minutes}m` : '';
            const secondsString = `${remainingSeconds}s`;

            return `${minutesString} ${secondsString}`;
        } else {
            return null;
        }
    })

    const setTimerSeconds = inject('setTimerSeconds');
    const selectPart = inject('selectPart');

    function activatePart() {
        if (props.part.timed) {
            setTimerSeconds(props.part.consumed);
            selectPart(props.part.id);
        }
    }

    function getImageUrl(name) {
        return new URL(`../assets/svg/${name}`, import.meta.url).href
    }
</script>
