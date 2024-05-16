<template>
    <div id="popup-bg">
        <div class="popup shake">
            <div class="title">
                <div>Meeting Timer</div>
                <div class="next">Next Part</div>
            </div>
            <div class="message">
                <div class="part-detail">
                    <div class="part">
                        {{ getPartName }}
                    </div>
                    <div class="pop-participant">
                        <div class="center">{{ getParticipant }}</div>
                        <div class="center next-duration">
                            <span>
                                {{ getDuration }}
                            </span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="actions">
                <button @click="startNextPart">Start</button>
                <button @click="setPopup(false)">Cancel</button>
            </div>
        </div>
    </div>
</template>

<script setup>
    import { computed, inject } from "vue";

    const props = defineProps({
        part: Object,
    });

    const getPartName = computed(() => {
        return props.part ? props.part.part : null;
    });

    const getParticipant = computed(() => {
        return props.part ? props.part.participant : null;
    });

    const getDuration = computed(() => {
        if (props.part) {
            return props.part.limit ? props.part.limit + "min" : "";
        } else {
            return null;
        }
    });

    const setPopup = inject('setPopup')
    const selectPart = inject('selectPart')
    const selectNextPart = inject('selectNextPart')
    const start = inject('start')

    async function startNextPart() {
        setPopup(false);
        await selectPart(props.part.id);
        await start();
        await selectNextPart();
    }
</script>
