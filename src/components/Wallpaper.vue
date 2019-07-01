<template>
  <div :class="['wallpaper', classList]">
    <img :src="imageSrc"
         class="wallpaper__img"
         ref="Image"
         alt="Unsplash Random Wallpaper">
  </div>
</template>

<script>
export default {
  name    : 'Wallpaper',
  data() {
    return {
      isLoading: false,
    }
  },
  computed: {
    classList() {
      return {
        'wallpaper--loading': this.isLoading,
        'wallpaper--ready'  : !this.isLoading,
      };
    },
    imageSrc() {
      return `https://source.unsplash.com/random/${screen.width}x${screen.height}?nature,travel,architecture`;
    },
  },
  mounted() {
    if (!this.$refs.Image.complete) {
      this.isLoading = true;
      this.$refs.Image.addEventListener('load', () => {
        this.isLoading = false;
      });
    }
  },
};
</script>

<style lang="scss">
@keyframes wallpaper__img--infinite {
  50% {
    transform : scale(2);
  }
}

.wallpaper {
  position        : absolute;
  top             : 0;
  left            : 0;
  right           : 0;
  bottom          : 0;
  display         : flex;
  align-items     : center;
  justify-content : center;
  overflow        : hidden;
  z-index         : 0;
  opacity         : 1;
  background      : #000000;
  transition      : opacity 3000ms ease;

  &__img {
    flex       : 0 0 auto;
    min-width  : 100%;
    min-height : 100%;
    opacity    : .1;
  }

  &--loading {
    opacity : 0;
  }

  &--ready &__img {
    animation : wallpaper__img--infinite 240s linear infinite;
  }
}
</style>
