<template>
  <div class="timeline-container">
    <TimelineItem
      v-for="(story, index) in stories"
      :key="index"
      :progress="itemProgress(index)"
    />
  </div>
</template>

<script>
import TimelineItem from "./TimelineItem.vue";

export default {
  components: {
    TimelineItem,
  },
  props: {
    current: { type: Number, required: true },
    stories: { type: Array, required: true },
    progress: { type: Number, required: true },
  },
  methods: {
    itemProgress(index) {
      if (index === this.current) {
        return this.progress;
      } else if (index < this.current) {
        return 100;
      }

      return 0;
    },
  },
};
</script>

<style scoped>
.timeline-container {
  position: absolute;
  display: flex;
  top: 0;
  padding: 20px;
  box-sizing: border-box;
  width: 100%;
}
</style>
