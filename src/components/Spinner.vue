<template>
  <div class="spinner">
    <div class="spinner__dot"></div>
    <div class="spinner__dot"></div>
    <div class="spinner__dot"></div>
  </div>
</template>

<script>
export default {
  name: 'Spinner',
}
</script>

<style lang="scss">
@keyframes spinner__dot--infinite {
  40% {
    transform : translateY(-5px);
  }
}

.spinner {
  display        : inline-block;
  vertical-align : middle;
}

.spinner > * {
  display       : inline-block;
  border-radius : 50%;
  background    : rgba(255, 255, 255, .5);
  width         : 3px;
  height        : 3px;
  margin        : 0 2px;
  animation     : spinner-dot__infinite 1.2s ease infinite;



  &:nth-child(1) {
    animation-delay : 0ms;
  }

  &:nth-child(2) {
    animation-delay : 120ms;
  }

  &:nth-child(3) {
    animation-delay : 240ms;
  }
}
</style>
