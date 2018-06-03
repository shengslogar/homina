<template>
    <div class="weather-container">

        <transition name="slideYFade">

            <!-- Weather loading... -->
            <Spinner v-if="coords.updated === null || weather.updated === null" class="weather-spinner"/>

            <div v-else class="weather">

                <div class="weather-now">
                <span class="weather-now-temp">
                    {{ Math.round(weather.temperature * 10)/10 }}
                </span>
                    <span class="weather-now-units">
                    &deg;{{ weather.units }}
                </span>
                </div>
                <div class="weather-location">
                <span class="weather-location-city">
                    {{ weather.location }}
                </span>
                    <span class="weather-location-coords">
                    ({{ Math.round(coords.long) }}, {{ Math.round(coords.lat) }})
                </span>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
    import Spinner from '@/components/Spinner';

    export default {
        name: 'Weather',
        components: {
            Spinner
        },
        props: {
            weatherUnits: {
                default: 'K',
                type: String
            },
            cache: null
        },
        data() {
            return {
                coords: {
                    lat: null,
                    long: null,
                    updated: null,
                    loading: true
                },
                weather: {
                    apiKey: 'd9b473a4fb6873a891ca291f10853854',
                    temperature: null,
                    units: null,
                    description: '',
                    location: '',
                    updated: null,
                    loading: true
                },
            };
        },
        methods: {

            async getWeather() {

                this.weather.loading = true;

                try {
                    const request = await fetch(`http://api.openweathermap.org/data/2.5/weather?lat=${this.coords.lat }&lon=${this.coords.long}&appid=${ this.weather.apiKey }`)
                        .then(response => response.json());

                    this.weather.rawTemp = request.main.temp;
                    this.weather.description = request.weather[0].description;
                    this.weather.location = request.name;
                    this.weather.loading = false;
                    this.weather.updated = (+new Date());


                    this.recomputeWeatherTemp();
                }
                catch (err) {
                    console.error('Error loading weather', err);
                }
            },
            recomputeWeatherTemp() {
                this.weather.units = this.weatherUnits;

                switch (this.weatherUnits) {
                    case 'F':
                        this.weather.temperature = (9 / 5) * (this.weather.rawTemp - 273) + 32
                        break;
                    case 'C':
                        this.weather.temperature = this.weather.rawTemp - 273;
                        break;

                    case 'K':
                    default:
                        this.weather.temperature = this.weather.rawTemp;
                        break;
                }
            },

            async getLocation() {

                this.coords.loading = true;

                await new Promise((resolve, reject) => {
                    navigator.geolocation.getCurrentPosition(
                        (response) => { // success
                            this.coords.lat = response.coords.latitude;
                            this.coords.long = response.coords.longitude;
                            this.coords.updated = +(new Date());
                            this.coords.loading = false;

                            if (resolve)
                                resolve.apply(this.coords);
                        },
                        (error) => { // failure

                            this.coords.loading = false;

                            if (reject)
                                reject.apply();

                            console.error('GeoLocationApi', error);
                        });
                });
            },

            async refreshWeather() {
                await this.getLocation();
                await this.getWeather();

                this.$emit('updateCache', {
                    coords: this.coords,
                    weather: this.weather
                });

                setTimeout(() => {
                    this.refreshWeather();
                }, 60000);
            }
        },
        watch: {
            weatherUnits() {
                this.recomputeWeatherTemp();
            }
        },
        created() {
            // cached data?
            if (this.cache !== null) {

                this.weather = Object.assign(this.weather, this.cache.weather);
                this.recomputeWeatherTemp(); // just in case, fringe case where setting changed but cache didn't update
                this.coords = Object.assign(this.coords, this.cache.coords);

                // wait a second -> save some api limits!
                setTimeout(() => {
                    this.refreshWeather();
                }, 1000);
            }
            else {
                this.refreshWeather();
            }
        }
    }
</script>

<style scoped>
    .slideYFade-enter-active,
    .slideYFade-leave-active {
        transition-duration : .4s;
    }

    .slideYFade-enter,
    .slideYFade-leave-to {
        opacity   : 0;
        transform : translateY(10px);
    }

    @keyframes weather-container-before--entrance {
        0% {
            /*transform : scaleX(1.2);*/
            opacity : 0;
        }
    }

    .weather-container {
        text-align : center;
        padding    : 40px 0;
        margin-top : 20px;
        height     : 150px;
        position   : relative;
    }

    .weather-container:before {
        content    : '';
        display    : block;
        position   : absolute;
        background : rgba(255, 255, 255, 0.5);
        height     : 2px;
        top        : -1px;
        left       : 0;
        right      : 0;
        animation  : weather-container-before--entrance 2.5s ease;

    }

    .weather > * {
        display        : inline-block;
        vertical-align : middle;
        width          : 50%;
    }

    .weather-spinner {
        position        : absolute;
        top             : 0;
        bottom          : 0;
        display         : flex;
        align-items     : center;
        justify-content : center;
        left            : 0;
        right           : 0;
    }

    .weather-now {
        font-size : 1rem;
    }

    .weather-now-temp {
        font-size : 3rem;
    }

    .weather-now-units {
        vertical-align : top;
        margin-top     : 10px;
        margin-left    : -5px;
        display        : inline-block;
    }

    .weather-location {
        font-size   : 1rem;
        border-left : 1px solid rgba(255, 255, 255, 0.5);
    }

    .weather-location-coords {
        opacity : 0.5;
    }

</style>
