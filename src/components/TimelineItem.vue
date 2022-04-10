<script setup>
defineProps({
  progress: { type: Number, required: true },
});
</script>

<template>
  <div class="timeline-item">
    <div class="progress" :style="{ width: `${progress}%` }" />
  </div>
</template>

<style scoped>
.timeline-item {
  height: 5px;
  background-color: #939393;
  border-radius: 5px;
  margin-right: 5px;
  flex-grow: 1;
}

.timeline-item:last-child {
  margin-right: 0;
}

.progress {
  height: 5px;
  border-radius: 5px;
  background-color: #d9d5d5;
}
</style>
