<template>
  <div>
    <div v-for="(timer, index) in timers" :key="timer.id">
      <div class="display">
        <input class="input-style" type="text" v-model="timer.title" />
        <input class="input-style" type="number" v-model="timer.time" />
      </div>
      <div>
        <button @click="startTimer(timer.id)">Start Timer</button>
        <button @click="pauseTimer(timer.id)">Pause Timer</button>
        <button @click="resetTimer(timer.id)">Reset Timer</button>
        <button @click="removeTimer(index)">Remove Timer</button>
        <button @click="toggleCountDirection(timer.id)">Count Up/Down</button>
      </div>
    </div>
    <button @click="addTimer">Add Timer</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timers: [
        {
          id: 1,
          title: "Timer 1",
          time: 0,
          startTime: null,
          interval: null,
          isRunning: false,
          isCountingUp: true,
          countDirection: true,
        },
      ],
      nextTimerId: 2,
    };
  },
  methods: {
    toggleCountDirection(id) {
      const timer = this.timers.find((timer) => timer.id === id);
      timer.countDirection = !timer.countDirection;
      console.log(timer.countDirection);
    },
    addTimer() {
      this.timers.push({
        id: this.nextTimerId,
        title: `Timer ${this.nextTimerId}`,
        time: 0,
        startTime: null,
        interval: null,
        isRunning: false,
        isCountingUp: true,
      });
      this.toggleCountDirection(this.nextTimerId);
      this.nextTimerId++;
    },
    removeTimer(index) {
      this.timers.splice(index, 1);
    },
    startTimer(id) {
      const timer = this.timers.find((timer) => timer.id === id);

      if (timer.isRunning) return;

      timer.startTime = Date.now();
      if (timer.countDirection) {
        timer.interval = setInterval(() => {
          timer.time += timer.isCountingUp ? 1 : -1;
        }, 1000);
      } else {
        timer.interval = setInterval(() => {
          timer.time -= timer.isCountingUp ? 1 : -1;
          if (timer.time <= 0) return this.resetTimer(id);
        }, 1000);
      }

      timer.isRunning = true;
    },
    pauseTimer(id) {
      const timer = this.timers.find((timer) => timer.id === id);

      if (!timer.isRunning) return;

      clearInterval(timer.interval);
      timer.interval = null;
      timer.isRunning = false;
    },
    resetTimer(id) {
      const timer = this.timers.find((timer) => timer.id === id);

      clearInterval(timer.interval);
      timer.interval = null;
      timer.isRunning = false;
      timer.time = 0;
    },
  },
};
</script>

<style>
.input-style {
  width: 200px;
  height: 40px;
  font-size: 18px;
  color: blue;
}
</style>
