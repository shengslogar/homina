<template>
    <div id="new-tab" :style="{ backgroundColor: storage.settings.backgroundColor }">
        <div id="new-tab-inner">
            <Clock :showSeconds="false" :militaryTime="storage.settings.clock.militaryTime" @click.native="toggleClockMode"/>
            <Weather :weatherUnits="storage.settings.weather.units" :cache="storage.cache.weather" @click.native="toggleWeatherMode" @updateCache="updateWeatherCache"/>
            <Settings :storage="storage" @update="saveSettings"/>
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
                    Object.assign(this.storage, JSON.parse(savedSession));
                }
            },
            /**
             * settings panel updated, save changes
             */
            saveSettings(newSettings) {
                Object.assign(this.storage.settings, newSettings);
            }
        },
        components: {
            Clock,
            Weather,
            Settings
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
        position            : fixed;
        top                 : 0;
        left                : 0;
        right               : 0;
        bottom              : 0;
        display             : flex;
        align-items         : center;
        justify-content     : center;
        background          : #222;
        transition-duration : 3s; /* bg color transition */
    }

    #new-tab-inner {
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
