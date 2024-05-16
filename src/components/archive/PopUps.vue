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
                <button @click="startNextPart">
                    Start
                </button>
                <button @click="setPopup(false)">
                    Cancel
                </button>
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
        getPartName() {
            return (this.part) ? this.part.part : null;
        },
        getParticipant() {
            return (this.part) ? this.part.participant : null;
        },
        getDuration() {
            if (this.part) {
                return this.part.limit ? this.part.limit + 'min' : ''
            } else {
                return null
            }
        }
    },
    methods: {
        async startNextPart() {
            this.setPopup(false)
            await this.selectPart(this.part.id)
            await this.start()
            await this.selectNextPart()
        }
    },
    inject: [
        'setPopup', 'selectPart', 'selectNextPart', 'start', 'setControls'
    ]
}
</script>
