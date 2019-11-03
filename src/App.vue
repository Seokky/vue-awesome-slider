<template>
  <div id="app">
    <VueAwesomeSlider
      class="slider"
      :controls="false"
    />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import VueAwesomeSlider from '@/plugin/Main.vue';

@Component({
  components: {
    VueAwesomeSlider,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
body,
html {
  margin: 0;
}

.slider {
  margin: 0 auto;
  margin-top: 100px;
  max-width: 700px;
}
</style>
