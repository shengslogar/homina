<template>
  <div class="clock">
    <span class="clock__hour">
      {{ time.hours }}
    </span>
    <span class="clock__separator"/>
    <span class="clock__minute">
      {{ time.minutes }}
    </span>

    <template v-if="hasSeconds">
      <span class="clock__separator"/>
      <div class="clock__second">
        {{ time.seconds }}
      </div>
    </template>

    <div v-if="!isMilitary"
         :class="['clock__ap', apClassList]">
      <span class="clock__ap-am">
        AM
      </span>
      <span class="clock__ap-pm">
        PM
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name    : 'Clock',
  props   : {
    hasSeconds: {
      default: false,
      type   : Boolean,
    },
    isMilitary: {
      default: false,
      type   : Boolean,
    },
  },
  data() {
    return {
      time : {
        hours  : 0,
        minutes: 0,
        seconds: 0,
        isPM   : null,
      },
      timer: null,
    };
  },
  computed: {
    apClassList() {
      return {
        'clock__ap--am': !this.time.isPM,
        'clock__ap--pm': this.time.isPM,
      };
    },
  },
  methods : {
    formatClockDigits(value) {
      if (value.toString().length < 2) {
        return `0${value}`;
      }
      return `${value}`;
    },
    updateTime() {
      const now        = new Date();
      const nowHours   = now.getHours();
      const nowMinutes = now.getMinutes();
      const nowSeconds = now.getSeconds();

      if (this.isMilitary) {
        this.time.hours = this.formatClockDigits(nowHours);
      } else {
        this.time.hours = nowHours > 12
          ? nowHours - 12
          : (nowHours === 0 ? 12 : nowHours);
      }
      this.time.minutes = this.formatClockDigits(nowMinutes);
      this.time.seconds = this.formatClockDigits(nowSeconds);

      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.updateTime();
      }, 1000 - now.getMilliseconds());
    },
  },
  watch   : {
    hasSeconds() {
      this.updateTime();
    },
    isMilitary() {
      this.updateTime();
    },
  },
  created() {
    this.updateTime();
  },
}
</script>

<style lang="scss">
@keyframes clock--entrance {
  from {
    opacity   : 0;
    transform : translateY(1rem);
  }
}

.clock {
  display         : flex;
  align-items     : center;
  justify-content : center;
  color           : rgba(255, 255, 255, .9);
  text-align      : center;
  animation       : clock--entrance 300ms ease;
  position        : relative;
  z-index         : 10;

  &__hour,
  &__minute,
  &__second,
  &__separator,
  &__ap {
    display         : flex;
    flex-direction  : column;
    justify-content : center;
    align-items     : center;
    height          : 7rem;
    text-shadow     : 0 0 .25rem rgba(0, 0, 0, .25);
  }

  &__hour,
  &__minute,
  &__second {
    font-size : 10rem;
  }

  &__hour {
    padding : 0 1rem;
  }

  &__minute,
  &__second {
    width : 14rem;
  }

  &__separator {
    width    : 2rem;
    position : relative;
    opacity  : .8;

    &::before,
    &::after {
      content    : "";
      background : #ffffff;
      height     : 10px;
      width      : 10px;
      margin     : 1rem 0;
    }
  }

  &__ap {
    justify-content : flex-end;

    &-am,
    &-pm {
      font-size  : .8rem;
      opacity    : .25;
      margin-top : .5rem;
    }

    &--pm &-pm {
      opacity : 1;
    }

    &--am &-am {
      opacity : 1;
    }
  }
}
</style>
