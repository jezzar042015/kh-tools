<template>
    <div class="preview">
        <div class="time">
            <span> {{ getDuration }} </span>
        </div>
        <div class="preview-partname">{{ setPartName }} </div>

    </div>
</template>

<script setup>
    import { computed } from 'vue';

    const props = defineProps({
        part: { type: Object, required: false }
    });

    const setPartName = computed(() => {
        return (props.part) ? props.part.part : null;
    });

    const getDuration = computed(() => {
        if (props.part) {
            return props.part.limit ? props.part.limit + 'm' : '';
        } else {
            return null
        }
    })
</script>
