<template>
    <div id="app">
        <NewTab/>
    </div>
</template>

<script>

    import NewTab from './components/NewTab.vue'

    export default {
        name: 'app',
        components: {
            NewTab
        }
    }
</script>

<style>
    * {
        box-sizing          : border-box;
        transition          : .2s ease;
        transition-property : opacity, transform, visibility;
    }

    html {
        margin      : 0;
        font        : 16px 'Montserrat', sans-serif;
        user-select : none;
        cursor      : default;
        height      : 100%;
        background  : linear-gradient(163deg, #4f65ff, #4256e2);
    }

    body {
        font      : inherit; /* chrome injects styles that would otherwise override */
        font-size : 0;
        margin    : 0;
        color     : #fff;
    }

    #app {
        display         : flex;
        align-items     : center;
        justify-content : center;
        position        : fixed;
        top             : 0;
        left            : 0;
        right           : 0;
        bottom          : 0;
    }
</style>
