<template>
    <div id="sidebar">
        <div class="parts-holder">
            <div class="meetings">
                <div :class="['meeting', { 'active-meeting': activeMeeting === 'mwb' }]"
                    @click="$emit('switchMeeting', 'mwb')">
                    <div class="meeting-icon">
                        <img src="../assets/svg/mwb.svg" alt="">
                    </div>
                    <div>Christian Life</div>
                </div>
                <div :class="['meeting', { 'active-meeting': activeMeeting === 'wt' }]"
                    @click="$emit('switchMeeting', 'wt')">
                    <div class="meeting-icon">
                        <img src="../assets/svg/wt.svg" alt="">
                    </div>
                    <div>Public Meeting</div>
                </div>
            </div>
            <template v-for="part in props.parts" :key="part.id">
                <PartItem :part="part"></PartItem>
            </template>
        </div>
    </div>
</template>

<script setup>
    import PartItem from './PartItem.vue';

    const props = defineProps({
        parts: { type: Array },
        activeMeeting: { type: String }
    });
</script>
