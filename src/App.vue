<template>
  <div class="container">
    <PomodoroTimer />
  </div>
</template>

<script>
import PomodoroTimer from "./components/PomodoroTimer.vue";
export default {
  name: "App",
  components: {
    PomodoroTimer,
  },
};
</script>

<style>
.pomodoro-container {
  text-align: center;
  font-family: sans-serif;
}
ul {
  margin: 0px;
}
body {
  margin: 0px;
}
</style>
