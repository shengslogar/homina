<template>
  <div class="settings">
    <div class="settings-blur-overlay" :class="{ 'settings-blur-overlay--visible': visible }" @click="toggleMenu"></div>

    <div class="settings-toggle" @click="toggleMenu" :class="{ 'settings-toggle--active': visible }">
      <span></span><span></span><span></span>
    </div>

    <div class="settings-menu" :class="{ 'settings-menu--visible': visible }">
      <a @click="modals.colorPicker = true; toggleMenu(); ">Change Color</a>
      <a @click="modals.resetSettings = true; toggleMenu(); ">Reset All Data</a>
    </div>

    <!-- Modals -->

    <Modal id="settings-color-modal" :visible="modals.colorPicker" title="Choose Your Flavor"
           @action="setBackgroundColor(); modals.colorPicker = false;" @cancel="modals.colorPicker = false">
      <div style="text-align: center">
                <span class="settings-color-modal-swatch"
                      v-for="hash in colorPalette"
                      :style="{ backgroundColor: hash }"
                      :class="{ 'settings-color-modal-swatch--selected': (selectedColor === hash) }"
                      @click="selectedColor = hash; "></span>
      </div>
    </Modal>

    <Modal id="settings-reset-modal" :visible="modals.resetSettings" title="Reset All Data" @action="resetSettings"
           @cancel="modals.resetSettings = false">
      Make it like day one. We'll put you on a fresh slate. Ready to roll?
    </Modal>
  </div>
</template>

<script>

import Modal from '@/components/Modal';

export default {
  name: 'Settings',
  components: { Modal },
  props: {
    storage: null,
  },
  methods: {
    toggleMenu() {
      this.visible = !this.visible;
    },
    /**
     * bubbles settings back to main app
     */
    bubbleSettings() {
      this.$emit('update', this.settings);
    },
    setBackgroundColor() {
      this.settings.backgroundColor = this.selectedColor;
      this.bubbleSettings();

    },
    resetSettings() {
      this.modals.resetSettings = false;
      localStorage.clear();
      window.location.reload();
    },
  },
  data() {
    return {
      visible: false,
      // clears out any observers and grab clean unVued data
      settings: JSON.parse(JSON.stringify(this.storage.settings)),
      colorPalette: [
        '#bb4237',
        '#ff4f5d',
        '#ff724f',
        '#f29b5f',
        '#ffd04f',
        '#64cb88',
        '#00af7d',
        '#1e7f63',
        '#437179',
        '#4f9bff',
        '#4f65ff',
        '#ba4fff',
        '#ff4f89',
        '#ff4fae',
        '#c3c3c3',
        '#aaaaaa',
        '#373737',
        '#000000',
      ],
      selectedColor: this.storage.settings.backgroundColor,
      modals: {
        resetSettings: false,
        colorPicker: false,
      },
    }
  },
}
</script>

<style>
.settings {
  position : fixed;
  top      : 20px;
  right    : 20px;
  z-index  : 100;
}

.settings-blur-overlay {
  position   : fixed;
  top        : 0;
  left       : 0;
  right      : 0;
  bottom     : 0;
  z-index    : 10;
  visibility : hidden;
  opacity    : 0;
  background : rgba(0, 0, 0, 0.5);
}

.settings-blur-overlay--visible {
  opacity             : 1;
  visibility          : visible;
  transition-duration : 1s;
}

.settings-toggle {
  position       : sticky;
  font-size      : 20px;
  letter-spacing : 1px;
  font-weight    : bold;
  opacity        : 0.3;
  top            : 0;
  right          : 0;
  padding        : 10px 10px;
  display        : inline-block;
  cursor         : pointer;
  z-index        : 11;
}

.settings-toggle:before {
  content       : '';
  position      : absolute;
  top           : 0;
  left          : 0;
  right         : 0;
  bottom        : 0;
  border-radius : 10px;
  background    : #ffffff;
  opacity       : 0;
  transform     : scaleY(.5);
  z-index       : 0;
}

.settings-toggle:hover,
.settings-toggle--active {
  opacity : 1;
}

.settings-toggle--active:before {
  opacity   : 1;
  transform : none;
}

.settings-toggle > * {
  display       : block;
  border-radius : 50%;
  background    : #ffffff;
  width         : 3px;
  height        : 3px;
  margin        : 4px 0;
  position      : relative;
}

.settings-toggle--active > * {
  background-color : #000000;
}

.settings-menu {
  position         : fixed;
  top              : 70px;
  right            : 20px;
  background       : #ffffff;
  padding          : 12px 0;
  box-shadow       : 0 2px 5px rgba(0, 0, 0, 0.08);
  border-radius    : 10px;
  visibility       : hidden;
  opacity          : 0;
  transform        : scale(0.9);
  transform-origin : 100% 0;
  z-index          : 11;
}

.settings-menu.settings-menu--visible {
  visibility : visible;
  opacity    : 1;
  transform  : none;
}

.settings-menu > a {
  font-size : .9rem;
  color     : #000000;
  padding   : 10px 30px;
  cursor    : pointer;
  width     : 220px;
  display   : block;
}

.settings-menu > a:hover {
  background-color : #dddddd;
}

/* */
.settings-color-modal-swatch {
  width         : 40px;
  height        : 40px;
  border-radius : 50%;
  display       : inline-block;
  margin        : 4px;
  cursor        : pointer;
  border        : 3px solid transparent;
}

.settings-color-modal-swatch--selected {
  transform    : scale(1.1);
  border-color : rgba(255, 255, 255, 0.4);
}
</style>
