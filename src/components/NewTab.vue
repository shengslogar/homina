<template>
  <div id="new-tab" :style="{ backgroundColor: storage.settings.backgroundColor }">

    <Settings :storage="storage" @update="saveSettings"/>

    <div id="new-tab-inner">
      <Clock :showSeconds="false" :militaryTime="storage.settings.clock.militaryTime" @click.native="toggleClockMode"/>
      <Weather :weatherUnits="storage.settings.weather.units" :cache="storage.cache.weather"
               @click.native="toggleWeatherMode" @updateCache="updateWeatherCache"/>
    </div>
  </div>
</template>

<script>
import Clock from '@/components/Clock';
import Weather from '@/components/Weather';
import Settings from '@/components/Settings';

export default {
  name: 'NewTab',
  props: {},
  data() {
    return {
      storage: {
        settings: {
          backgroundColor: '#4f65ff',
          clock: {
            militaryTime: false,
          },
          weather: {
            units: 'F',
          },
          storageKey: 'homina-app',
        },
        cache: {
          weather: null,
        },
      },
    }
  },
  watch: {
    storage: {
      handler() {
        // save settings to storage
        localStorage.setItem(this.storage.settings.storageKey, JSON.stringify(this.storage));
      },
      deep: true,
    },
  },
  methods: {
    toggleClockMode() {
      this.storage.settings.clock.militaryTime = !this.storage.settings.clock.militaryTime;
    },
    toggleWeatherMode() {
      this.storage.settings.weather.units = (this.storage.settings.weather.units == 'F' ? 'C' : (this.storage.settings.weather.units == 'C' ? 'K' : 'F'));
    },
    updateWeatherCache(cachedData) {
      this.storage.cache.weather = cachedData;
    },
    attemptSessionLoad() {
      // load settings
      const savedSession = localStorage.getItem(this.storage.settings.storageKey);

      if (savedSession !== null) {
        Object.assign(this.storage, JSON.parse(savedSession));
      }
    },
    /**
     * settings panel updated, save changes
     */
    saveSettings(newSettings) {
      Object.assign(this.storage.settings, newSettings);
    },
  },
  components: {
    Clock,
    Weather,
    Settings,
  },

  beforeMount() {
    this.attemptSessionLoad();

    // reload saved settings on window focus
    window.addEventListener('focus', () => {
      this.attemptSessionLoad();
    });
  },
}
</script>

<style scoped>
#new-tab {
  position            : fixed;
  top                 : 0;
  left                : 0;
  right               : 0;
  bottom              : 0;
  display             : flex;
  background          : #222222;
  /* background color transition */
  transition-duration : 3s;
  overflow            : auto;
}

#new-tab-inner {
  width   : 600px;
  flex    : 0 0 auto;
  margin  : auto;
  padding : 4rem;
}

@keyframes new-tab-component--entrance {
  from {
    opacity   : 0;
    transform : translateY(10px);
  }
}

#new-tab-inner > * {
  animation : new-tab-component--entrance .5s ease;
}

</style>
