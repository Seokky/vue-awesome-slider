<template>
  <section class="vueas-slide">
    <slot>Slide</slot>
  </section>
</template>

<style lang="scss">
.vueas-slide {
  border-radius: 2px;
  box-shadow: 0 0 6px rgba(black, 0.3);
  margin: 0 5px;
  padding: 10px;
  min-height: 100px;
  min-width: 240px;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}
</style>
