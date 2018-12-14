<template>
  <div class="spinner">
    <div class="spinner-dot-one"></div>
    <div class="spinner-dot-two"></div>
    <div class="spinner-dot-three"></div>
  </div>
</template>

<script>
export default {
  name: 'Spinner',
}
</script>

<style scoped>
@keyframes spinner-dot--bounce {
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
  background    : rgba(255, 255, 255, 0.5);
  width         : 3px;
  height        : 3px;
  margin        : 0 2px;
  animation     : spinner-dot--bounce 1.2s ease infinite;
}

.spinner-dot-one {
  animation-delay : 0s;
}

.spinner-dot-two {
  animation-delay : .12s;
}

.spinner-dot-three {
  animation-delay : .24s;
}
</style>
