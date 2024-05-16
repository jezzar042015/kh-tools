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
            <template v-for="part in parts" :key="part.id">
                <PartItem :part="part"></PartItem>
            </template>
        </div>
    </div>
</template>

<script>
import PartItem from './ZPartItem.vue';
export default {
    name: 'SideBar',
    components: {
        PartItem
    },
    props: {
        parts: Array,
        activeMeeting: String
    },
    computed: {

    },
    methods: {

    },

}
</script>

<style>
#sidebar
{
    position: absolute;
    top: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.836);
    height: 100dvh;
    min-width: 400px;

    display: flex;
    align-items: center;
}

/* on phones */
@media screen and (max-width: 767px)
{
    #sidebar
    {
        top: 300px;
        height: calc(100dvh - 300px);
        min-width: 0;
    }
}
</style>

<style scoped>
.parts-holder
{
    padding: 0px 0px 0px 0px;
    height: 100%;
    width: 100%;
    display: flex;
    flex-flow: column;
    overflow-y: scroll;
}

.meetings
{
    display: flex;
    width: 100%;
    height: 60px;
    background: rgba(245, 245, 245, 0.301);
}

.meeting
{
    height: 100%;
    width: 50%;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 3px;
    cursor: pointer;
    user-select: none;
}

.meeting-icon
{
    width: 60px;
    height: 100%;
    display: flex;
    align-items: center;
}

.meeting-icon img
{
    height: 60px;

}

.parts-holder::-webkit-scrollbar
{
    width: 2px;
}

.active-meeting
{
    background: white;
}
</style>

