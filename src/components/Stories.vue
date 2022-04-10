<script setup>
import Timeline from "./Timeline.vue";
import { ref, onMounted, computed, toRefs } from "vue";

const props = defineProps({
  stories: { type: Object, required: true },
});

const current = ref(0);
const currentProgress = ref(0);
const running = ref(true);

const shouldNavigateRight = computed(() => {
  return current.value < props.stories.length - 1;
});

const shouldNavigateLeft = computed(() => {
  return current.value > 0;
});

const onRightNavigation = () => {
  if (shouldNavigateRight.value) {
    current.value++;
    currentProgress.value = 0;
    running.value = true;
  }
};

const onLeftNavigation = () => {
  if (shouldNavigateLeft.value) {
    current.value--;
    currentProgress.value = 0;
    running.value = true;
  }
};

const runProgress = () => {
  if (currentProgress.value >= 100) {
    if (current.value < props.stories.length - 1) {
      currentProgress.value = 0;
      current.value++;
    }
  } else if (running.value) {
    currentProgress.value += 0.5;
  }
  requestAnimationFrame(runProgress, 100);
};

onMounted(() => {
  window.addEventListener("keydown", (event) => {
    const RIGHT_ARROW = 39;
    const LEFT_ARROW = 37;
    const SPACE = 32;

    if (event.keyCode === RIGHT_ARROW) {
      onRightNavigation();
    }

    if (event.keyCode === LEFT_ARROW) {
      onLeftNavigation();
    }

    if (event.keyCode === SPACE) {
      running.value = !running.value;
    }
  });

  runProgress();
});
</script>

<script>
import Story1 from "./Story1.vue";
import Story2 from "./Story2.vue";
import Story3 from "./Story3.vue";

export default {
  components: {
    Story1,
    Story2,
    Story3,
  },
};
</script>

<template>
  <div class="stories">
    <Timeline
      :current="current"
      :stories="stories"
      :progress="currentProgress"
    />
    <div class="arrow left" @click="onLeftNavigation" v-if="shouldNavigateLeft">
      &lt;
    </div>
    <div class="story-wrapper">
      <component
        :is="stories[current].template"
        @click.native="running = !running"
      />
    </div>
    <div
      class="arrow right"
      @click="onRightNavigation"
      v-if="shouldNavigateRight"
    >
      &gt;
    </div>
  </div>
</template>

<style scoped>
.stories {
  display: flex;
  height: 100%;
  width: 500px;
  margin: 0 auto;
  flex: 1;
  background-color: white;
}

.stories {
  position: relative;
}

.story-wrapper {
  display: flex;
  flex-grow: 1;
  flex-direction: column;
  border-radius: 10px;
  height: 100%;
}

.arrow {
  position: absolute;
  height: 100%;
  display: flex;
  align-items: center;
  color: white;
  font-size: 36px;
  cursor: pointer;
}

.arrow.left {
  left: -40px;
}

.arrow.right {
  right: -40px;
}

@media only screen and (max-width: 500px) {
  .stories {
    width: 100%;
  }
  .arrow {
    display: none;
  }
}
</style>
