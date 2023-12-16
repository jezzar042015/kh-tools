<template>
    <div class="preview">
        <div class="time">
            <span>
                {{ getDuration }}
            </span>
        </div>
        <div class="partname">{{ setPartName }} </div>
        
    </div>
</template>

<script>
export default {
    props: {
        part: Object
    },
    computed: {
        setPartName() {
            return (this.part) ? this.part.part : null;
        },
        getDuration() {
            if (this.part) {
                return this.part.limit ? this.part.limit + 'm' : ''
            } else {
                return null
            }
        }
    }
}
</script>

<style scoped>
.preview {
    padding: 15px;
    background: transparent;
    position: absolute;

    display: flex;
    align-items: center;
}
.partname {
    color: white;
    font-size: 14px;
}

.time
{
    margin: 10px;
    border: 2px solid orange;
    border-radius: 50%;
    height: 35px;
    width: 35px;
    display: flex;
    justify-content: center;
    align-items: center;
    
}

.time span {
    font-size: 9px;
    color: white;
}
</style>