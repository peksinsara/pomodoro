<template>
  <div>
    <div class="theme-selector">
      <ul>
        <li
          v-for="(theme, index) in themes"
          :key="index"
          @click="selectTheme(theme.value)"
          :class="{ active: theme.value === selectedTheme }"
        >
          {{ theme.label }}
        </li>
      </ul>
    </div>

    <div class="pomodoro" :style="themeStyles">
      <div class="action-buttons">
        <button
          @click="setDuration(25)"
          :class="{ active: selectedDuration === 25 }"
          :style="getButtonStyles(25)"
        >
          Pomodoro
        </button>
        <button
          @click="setDuration(5)"
          :class="{ active: selectedDuration === 5 }"
          :style="getButtonStyles(5)"
        >
          Short Break
        </button>
        <button
          @click="setDuration(15)"
          :class="{ active: selectedDuration === 15 }"
          :style="getButtonStyles(15)"
        >
          Long Break
        </button>
      </div>
      <h1>{{ formatTime(timer.minutes) }}:{{ formatTime(timer.seconds) }}</h1>
      <button
        @click="startTimer"
        v-if="!timerRunning"
        :style="getButtonStyles(selectedDuration)"
      >
        Start
      </button>
      <button
        @click="stopTimer"
        v-else
        :style="getButtonStyles(selectedDuration)"
      >
        Stop
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PomodoroTimer",
  data() {
    return {
      timer: {
        minutes: 25,
        seconds: 0,
      },
      timerRunning: false,
      intervalId: null,
      selectedTheme: "default",
      selectedDuration: null,
      themes: [
        { label: "Default", value: "default" },
        { label: "Dust Pink", value: "dustPink" },
        { label: "Pastel Blue", value: "pastelBlue" },
        { label: "Synthwave", value: "synthPurple" },
      ],
    };
  },
  computed: {
    themeStyles() {
      switch (this.selectedTheme) {
        case "dustPink":
          return {
            backgroundImage:
              "url('https://i.pinimg.com/564x/90/c9/dd/90c9dd8386bc6394861465924618d6fb.jpg')",
            backgroundSize: "cover",
            color: "#383838",
          };
        case "pastelBlue":
          return {
            backgroundImage:
              "url('https://i.pinimg.com/564x/46/40/ce/4640ce518280885a19ade78b9bbd10c5.jpg')",
            backgroundSize: "cover",
            color: "#DDD8D0",
          };

        case "synthPurple":
          return {
            backgroundImage:
              "url('https://i.pinimg.com/564x/02/1d/00/021d0062a457bd7f029e52b8506f442b.jpg')",
            backgroundSize: "cover",
            color: "#DDD8D0",
          };

        default:
          return {
            backgroundColor: "#353535",
            color: "#BCA285",
          };
      }
    },
  },
  methods: {
    setDuration(duration) {
      this.selectedDuration = duration;
      this.timer.minutes = duration;
      this.timer.seconds = 0;
    },
    startTimer() {
      if (!this.timerRunning && this.selectedDuration) {
        this.timerRunning = true;
        this.intervalId = setInterval(this.tick, 1000);
      }
    },
    stopTimer() {
      this.timerRunning = false;
      clearInterval(this.intervalId);
    },
    formatTime(time) {
      return time < 10 ? "0" + time : time;
    },
    tick() {
      if (this.timer.seconds > 0) {
        this.timer.seconds--;
      } else if (this.timer.minutes > 0) {
        this.timer.minutes--;
        this.timer.seconds = 59;
      } else {
        this.timerRunning = false;
        clearInterval(this.intervalId);
        alert("Pomodoro session completed!");
      }
    },
    selectTheme(theme) {
      this.selectedTheme = theme;
    },
    getButtonStyles(duration) {
      const baseStyles = {
        fontSize: "1.5em",
        marginTop: "20px",
        padding: "10px 20px",
        border: "none",
        borderRadius: "18px",
        cursor: "pointer",
        fontFamily: this.getButtonFontFamily(),
        color:
          this.selectedTheme === "pastelBlue" ||
          this.selectedTheme === "synthPurple"
            ? "#fff"
            : "inherit",
      };

      const activeStyles = {
        backgroundColor:
          duration === this.selectedDuration
            ? this.getActiveThemeColor()
            : "inherit",
      };

      const inactiveStyles = {
        border:
          duration !== this.selectedDuration
            ? `1px solid ${this.getInactiveThemeColor()}`
            : "none",
        backgroundColor:
          duration !== this.selectedDuration ? "transparent" : "inherit",
      };

      return {
        ...baseStyles,
        ...(duration === this.selectedDuration ? activeStyles : inactiveStyles),
      };
    },
    getActiveThemeColor() {
      switch (this.selectedTheme) {
        case "dustPink":
          return "#9E898E";
        case "pastelBlue":
          return "#495768";
        case "synthPurple":
          return "#792C76";
        default:
          return "#717A5E";
      }
    },
    getInactiveThemeColor() {
      switch (this.selectedTheme) {
        case "dustPink":
          return "#9E898E";
        case "pastelBlue":
          return "#495768";
        case "synthPurple":
          return "#792C76";
        default:
          return "#717A5E";
      }
    },
    getButtonFontFamily() {
      switch (this.selectedTheme) {
        case "dustPink":
          return "Playfair Display";
        case "pastelBlue":
          return "Playfair Display";
        case "synthPurple":
          return "Space Mono";
        default:
          return "Montserrat";
      }
    },
  },
};
</script>

<style scoped>
.pomodoro {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding: 0;
  padding-top: 20%;
  height: 100vh;
}
h1 {
  font-size: 5em;
}
.theme-selector {
  margin-top: 20px;
  color: white;
  font-size: 18px;
  position: absolute;
  width: 100%;
  border-bottom: 1px solid black;
}
.theme-selector ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  font-size: 20px;
}
.theme-selector li {
  cursor: pointer;
  padding: 10px;
  margin-right: 10px;
  border-radius: 5px;
}
.theme-selector li.active {
  background-color: rgba(0, 0, 0, 0.234);
  color: white;
}
.action-buttons {
  display: flex;
  flex-direction: row;
  margin: 20px;
}
.action-buttons button {
  margin: 0px 20px;
  border-radius: 30px;
}
</style>
