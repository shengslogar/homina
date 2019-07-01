<template>
  <div class="new-tab">
    <Wallpaper/>
    <Clock/>
  </div>
</template>

<script>
import Clock from './Clock';
import Wallpaper from './Wallpaper';

export default {
  name      : 'NewTab',
  components: { Wallpaper, Clock },
};
</script>

<style lang="scss">
@keyframes new-tab--entrance {
  from {
    opacity : 0;
  }
}

.new-tab {
  background      : #000000;
  position        : fixed;
  top             : 0;
  left            : 0;
  right           : 0;
  bottom          : 0;
  overflow        : auto;
  animation       : new-tab--entrance .1s ease;
  display         : flex;
  flex-direction  : column;
  align-items     : center;
  justify-content : center;
}
</style>
