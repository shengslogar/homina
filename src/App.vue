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
    NewTab,
  },
}
</script>

<style>
*, *:before, *:after {
  box-sizing          : border-box;
  transition          : .2s ease;
  transition-property : opacity, transform, visibility, border-color, background-color;
}

html {
  margin      : 0;
  font        : 16px 'Montserrat', sans-serif;
  user-select : none;
  cursor      : default;
  height      : 100%;
}

body {
  font      : inherit; /* chrome injects styles that would otherwise override */
  font-size : 0;
  margin    : 0;
  color     : #ffffff;
}

.btn {
  font-size     : 1rem;
  padding       : 10px 20px;
  border        : 1px solid #dddddd;
  background    : #ffffff;
  color         : #000000;
  display       : inline-block;
  margin        : 0 4px;
  border-radius : 20px;
  text-align    : center;
  cursor        : pointer;
}

.btn:hover {
  border-color : #cccccc;
}

#app {
  position : fixed;
  top      : 0;
  left     : 0;
  right    : 0;
  bottom   : 0;
}
</style>
