<template>
  <button class="button"
          @click.native="$emit('click')">
    <slot/>
  </button>
</template>

<script>
export default {
  name: 'Button',
};
</script>

<style lang="scss">
.button {
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
  outline       : none;

  &:hover,
  &:focus {
    border-color : #cccccc;
  }
}
</style>
