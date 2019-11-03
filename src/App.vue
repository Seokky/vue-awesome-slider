<template>
  <div id="app">
    <VueAwesomeSlider class="slider">
      <Slide>1</Slide>
      <Slide>2</Slide>
      <Slide>3</Slide>
      <Slide>4</Slide>
      <Slide>5</Slide>
    </VueAwesomeSlider>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { VueAwesomeSlider, Slide } from '@/plugin/index';

@Component({
  components: {
    VueAwesomeSlider, Slide,
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
