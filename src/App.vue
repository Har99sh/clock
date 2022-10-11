<template>
  <div>
    <button v-if="!is_running" @click="startTimer">Start timer</button>
    <button v-else @click="stopTimer">Stop timer</button>
    <h1>{{running_time}}</h1>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
    total_time: null,
    running_time: 0,
    start_time: null,
    finish_time: null,
    is_running: false,
    time_interval: null,
    }

  },
  methods: {
    startTimer() {
      // Send post request to api /start-timer
      this.is_running = true;
      this.start_time = Date.now() - this.running_time;
      this.time_interval = setInterval(()=> {
        this.running_time = Date.now() - this.start_time;
        this.calculateTime(this.running_time);
      }, 1000)
    },
    calculateTime(runningTime) {
          const total_seconds = Math.floor(runningTime / 1000);
          const total_minutes = Math.floor(total_seconds / 60);
          const total_hours = Math.floor(total_minutes / 60);

          const display_seconds = (total_seconds % 60).toString().padStart(2, "0");
          const display_minutes = total_minutes.toString().padStart(2, "0");
          const display_hours = total_hours.toString().padStart(2, "0");

          this.running_time =  `${display_hours}:${display_minutes}:${display_seconds}`
    },
    stopTimer(){
      //Send post request api to stop timer
      this.is_running = false;
      this.running_time = 0;
      clearInterval(this.time_interval);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
