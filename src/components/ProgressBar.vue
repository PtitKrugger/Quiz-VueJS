<template>
    <label>
        <progress :value="currentValue" :max="maximum" />
        {{ (currentValue / maximum) * 100 }}%
    </label>
</template>

<script setup>
defineProps({
    currentValue: Number,
    maximum: Number
})
</script>

<style scoped>
    progress {
        width: auto !important;
        margin-bottom: 2px !important;
    }
</style>