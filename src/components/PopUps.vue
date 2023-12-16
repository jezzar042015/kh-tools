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
                    <div class="participant">
                        <div class="center">{{ getParticipant }}</div>
                        <div class="center time">
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

<style scoped>
#popup-bg
{
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    width: 100%;
    height: 100%;
    background: rgba(36, 36, 36, 0.795);

    display: flex;
    justify-content: center;
    align-items: center;
}

.popup
{
    height: fit-content;
    width: fit-content;
    max-width: 700px;
    max-height: 500px;
    background: rgb(241, 241, 241);
    border-radius: 5px;
    box-shadow: 0px 10px 39px 10px #cecece38;
    padding: 20px;

    display: flex;
    flex-flow: column;
}

@media screen and (max-width: 767px)
{
    .popup
    {
        width: 95%;
    }
}

#start img
{
    height: 85%;
    background-color: transparent;
}

.title
{
    font-size: 12px;
    display: flex;
    justify-content: space-between;
    padding-bottom: 25px;
    gap: 50px;
}

.message
{
    flex: 1;
    display: flex;
    flex-flow: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    gap: 5px;
}

.next
{
    padding: 5px 10px;
    color: white;
    background: rgb(115, 202, 115);
    width: fit-content;
    border-radius: 5px;
    font-size: 10px;
    align-self: start;
}

.part-detail
{
    width: 100%;
    display: flex;
    flex-flow: column;
    justify-content: center;
    gap: 5px;
    padding: 0px 15px;
}

.part
{
    font-size: 20px;
    font-weight: 600;
    text-align: center;
}

.participant
{
    width: 100%;
    font-size: 14px;
    display: flex;
    flex-flow: column;
    justify-content: center;
    align-items: center;
}

.center
{
    text-align: center;
}

.time
{
    margin: 10px;
    border: 3px solid orange;
    border-radius: 50%;
    height: 80px;
    width: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.time span
{
    font-size: 12px;
}

.actions
{
    height: auto;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 15px 15px 0px 15px;
    gap: 10px;
}

.actions button
{
    border: none;
    padding: 15px 30px;
    border-radius: 5px;
    background: lightgrey;
    color: rgb(44, 42, 42);
    cursor: pointer;
    font-weight: 500;
}

.actions button:hover
{
    color: white;
    background: #2d2da3
}

.shake {
  animation: shake .5s infinite;
}

@keyframes shake {
    0% { transform: rotate(0deg); }
  25% { transform: rotate(-1deg); }
  50% { transform: rotate(1deg); }
  75% { transform: rotate(-1deg); }
  100% { transform: rotate(1deg); }
}

</style>