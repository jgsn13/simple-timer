<template>
  <div :class="containerClasses" :style="{height: '100vh'}">
    <h1>Timer</h1>
    <h2 :class="timerClasses" >{{ hour }}:{{ min }}:{{ sec }}</h2>
    <div>
      <button @click="start()" :disabled="startDisabled" class="btn btn-primary m-2">Start</button>
      <button @click="stop()" :disabled="isStoped" class="btn btn-danger m-2">Stop</button>
      <button @click="reset()" :disabled="cannotReset" class="btn btn-warning m-2">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Timer',
  data() {
    return {
      hour: '00',
      min: '00',
      sec: '00',
      counterHour: 0,
      counterMin: 0,
      counterSec: 0,
      startDisabled: false,
      isStoped: true,
      cannotReset: true,
      timerClasses: {
        'mt-2': true,
      }
    }
  },
  watch: {
    counterSec(value) {
      if (value === 60) {
        this.counterMin++;
        this.counterSec = 0;
      }
      this.sec = value >= 10 ? `${value}` : `0${value}`;
    },
    counterMin(value) {
      if (value === 60) {
        this.counterHour++;
        this.counterMin = 0;
      }
      this.min = value >= 10 ? `${value}` : `0${value}`;
    },
    counterHour(value) {
      if (value === 24) {
        this.counterHour = 0;
        this.counterMin = 0;
        this.counterSec = 0;
      }
      this.hour = value >= 10 ? `${value}` : `0${value}`;
    }
  },
  methods: {
    start() {
      this.startDisabled = true;
      this.isStoped = false;
      this.cannotReset = false;
      this.timerClasses['text-danger'] = false;
      this.initSec = setInterval(() => {
        this.counterSec++;
      }, 1000);
    },
    stop() {
      clearInterval(this.initSec);
      this.startDisabled = false;
      this.isStoped = true;
      this.timerClasses['text-danger'] = true;
    },
    reset() {
      clearInterval(this.initSec);
      this.isStoped = true;
      this.cannotReset = true;
      this.startDisabled = false;
      this.timerClasses['text-danger'] = false;
      this.counterHour = 0;
      this.counterMin = 0;
      this.counterSec = 0;
    }
  },
  computed: {
    containerClasses() {
      return {
        'container-xxl': true,
        'text-light': true,
        'bg-dark': true,
        'rounded-3': true,
        'd-flex': true,
        'flex-column': true,
        'justify-content-center': true,
        'align-items-center': true
      }
    }
  }
}
</script>

<style scoped>
</style>
