<template>
    <div id="new-tab">
        <Clock :showSeconds="false" :militaryTime="storage.settings.clock.militaryTime" @click.native="toggleClockMode"/>
        <Weather :weatherUnits="storage.settings.weather.units" :cache="storage.cache.weather" @click.native="toggleWeatherMode" @updateCache="updateWeatherCache"/>
    </div>
</template>

<script>
    import Clock from '@/components/Clock';
    import Weather from '@/components/Weather';

    export default {
        name: 'NewTab',
        props: {},
        data() {
            return {
                storage: {
                    settings: {
                        clock: {
                            militaryTime: false
                        },
                        weather: {
                            units: 'F',
                        },
                        storageKey: 'homina-app',
                    },
                    cache: {
                        weather: null
                    }
                }
            }
        },
        watch: {
            storage: {
                handler() {
                    // save settings to storage
                    localStorage.setItem(this.storage.settings.storageKey, JSON.stringify(this.storage));
                },
                deep: true
            },
            _storage: {
                handler() {
                    console.log('local storage update')
                },
                deep: true

            }
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
                    this.storage = JSON.parse(savedSession);
                }
            }
        },
        components: {
            Clock,
            Weather
        },

        beforeMount() {
            this.attemptSessionLoad();

            // reload saved settings on window focus
            window.addEventListener('focus', () => {
                this.attemptSessionLoad();
            });
        }
    }
</script>

<style scoped>
    #new-tab {
        width     : 90%;
        max-width : 500px;
        min-width : 400px;
    }

    @keyframes new-tab-component--entrance {
        from {
            opacity   : 0;
            transform : translateY(10px);
        }
    }

    #new-tab > * {
        animation : new-tab-component--entrance .5s ease;
    }

    @media all and (max-width : 600px) {
        #new-tab {
            transform : scale(0.8);
        }
    }

    @media all and (max-width : 400px) {
        #new-tab {
            transform : scale(0.5);
        }
    }

</style>
