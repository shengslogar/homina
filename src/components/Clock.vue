<template>
  <div class="clock">
    <div class="clock-hours">{{ time.hours }}</div>
    <div class="clock-separator"></div>
    <div class="clock-minutes">{{ time.minutes }}</div>

    <div v-if="showSeconds" class="clock-separator"></div>
    <div v-if="showSeconds" class="clock-seconds">{{ time.seconds }}</div>

    <div class="clock-ap" v-if="!militaryTime" :class="{ 'clock-ap--pm': time.pm }">
      <span class="clock-ap-a">AM</span>
      <span class="clock-ap-p">PM</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Clock',
  props: {
    showSeconds: {
      default: false,
      type: Boolean,
    },
    militaryTime: {
      default: false,
      type: Boolean,
    },
  },
  data() {
    return {
      time: {
        hours: 0,
        minutes: 0,
        seconds: 0,
        pm: null,
      },
      timerCache: null,
    };
  },
  methods: {
    updateTime() {
      const now = new Date();

      let h = now.getHours();
      let m = now.getMinutes();
      let s = now.getSeconds();
      const pm = h > 11;

      if (!this.militaryTime) {

        if (h > 12) {
          h -= 12;
        } else if (h == 0) {
          h = 12;
        }
      }

      if (m < 10) {
        m = '0' + m;
      }

      if (s < 10) {
        s = '0' + s;
      }

      this.time.hours = h.toString();
      this.time.minutes = m.toString();
      this.time.seconds = s.toString();
      this.time.pm = pm;

      // update at next tick
      clearTimeout(this.timerCache); // prevent any loops
      this.timerCache = setTimeout(() => {
        this.updateTime();
      }, 1000 - now.getMilliseconds());
    },
  },
  watch: {
    militaryTime() {
      this.updateTime();
    },
    showSeconds() {
      this.updateTime();
    },
  },

  created() {
    this.updateTime();
  },
}
</script>

<style scoped>
.clock {
  display    : block;
  text-align : center;
}

.clock > * {
  display        : inline-block;
  font-size      : 10rem;
  font-weight    : normal;
  vertical-align : baseline;
}

.clock-separator::before {
  content : ':';
}

.clock-separator {
  margin : 0 8px;
}

.clock-ap {
  margin-left : 10px;
}

.clock-ap > span {
  font-size  : 1rem;
  display    : block;
  margin-top : 4px;
  opacity    : .2;
}

.clock-ap:not(.clock-ap--pm) > .clock-ap-a,
.clock-ap.clock-ap--pm > .clock-ap-p {
  opacity : 1;
}
</style>
