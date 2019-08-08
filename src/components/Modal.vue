<template>
  <div class="modal" :class="{ 'modal--visible': visible, 'modal--transitioning': transitioning }"
       @transitionstart="handleTransitionStart"
       @transitionend="handleTransitionEnd">
    <div class="modal-body">
      <div class="modal-title">{{ title }}</div>
      <div class="modal-content">
        <slot></slot>
      </div>
      <div class="modal-buttons">
        <a class="btn btn--link" @click="$emit('cancel')">Cancel</a>
        <a class="btn" @click="$emit('action')">OK</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    title: null,
    content: null,
    visible: false,
  },
  data() {
    return {
      transitioning: false
    };
  },
  methods: {
    handleTransitionStart({target}) {
      if (target === this.$el) {
        this.transitioning = true;
      }
    },
    handleTransitionEnd({target}) {
      if (target === this.$el) {
        this.transitioning = false;
      }
    }
  }
}
</script>

<style scoped>
.modal {
  position            : fixed;
  top                 : 0;
  left                : 0;
  right               : 0;
  bottom              : 0;
  z-index             : 100;
  opacity             : 0;
  visibility          : hidden;
  display             : flex;
  justify-content     : center;
  background          : rgba(0, 0, 0, 0.5);
  transition-duration : .4s;
  overflow            : auto;
  padding             : 1rem 0;
}

.modal--visible {
  opacity    : 1;
  visibility : visible;
}

.modal--transitioning {
  overflow : hidden;
}

.modal-body {
  background          : #ffffff;
  border-radius       : 20px;
  overflow            : hidden;
  color               : #000000;
  width               : 520px;
  max-width           : 98%;
  box-shadow          : 0 5px 10px rgba(0, 0, 0, 0.2);
  transform           : scale(1.2);
  transition-duration : .3s;
  margin              : auto 0;
}

.modal--visible > .modal-body {
  transform : none;
}

.modal-title {
  font-size      : 1.4rem;
  font-weight    : bold;
  text-align     : center;
  text-transform : uppercase;
  padding        : 20px 10px;
  background     : #f6f6f6;
}

.modal-content {
  padding   : 30px;
  font-size : 1rem;
}

.modal-buttons {
  border-top : 1px solid #eeeeee;
  text-align : right;
  padding    : 12px 15px;
}
</style>
