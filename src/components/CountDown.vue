<script>
import { ref, computed, onMounted, onBeforeUnmount } from "vue";

export default {
  setup() {
    const targetTime = new Date("2023-12-31T23:59:59").getTime();
    const currentTime = ref(new Date().getTime());
    const timeRemaining = ref(targetTime - currentTime.value);

    const formattedTime = computed(() => {
      const seconds = Math.floor(timeRemaining.value / 1000);
      const minutes = Math.floor(seconds / 60);
      const hours = Math.floor(minutes / 60);
      const days = Math.floor(hours / 24);

      return `${days}d ${hours % 24}h ${minutes % 60}m ${seconds % 60}s`;
    });

    let intervalId;

    onMounted(() => {
      intervalId = setInterval(updateTimer, 1000);
    });

    onBeforeUnmount(() => {
      clearInterval(intervalId);
    });

    function updateTimer() {
      currentTime.value = new Date().getTime();
      timeRemaining.value = Math.max(0, targetTime - currentTime.value);
    }

    return {
      formattedTime,
    };
  },
};
</script>

<template>
  <div class="countdown">
    <div class="timer">
      {{ formattedTime }}
    </div>
  </div>
</template>

<style scoped>
.countdown {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: aqua;
}

.timer {
  font-size: 2rem;
  display: flex;
  align-items: center;
  padding: 1rem;
  background-color: #333;
  color: #fff;
  border-radius: 0.5rem;
}

.separator {
  margin: 0 0.5rem;
  color: #9edf36;
}
</style>
