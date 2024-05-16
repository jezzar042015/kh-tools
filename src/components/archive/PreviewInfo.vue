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
