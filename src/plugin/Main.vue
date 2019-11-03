<template>
  <section
    class="vueas-wrapper"
    @mousedown.capture="startDrag"
    @touchstart.capture="startDrag"
    @touchend.capture="stopDrag"
    @touchcancel.capture="stopDrag"
    @touchleave="stopDrag"
    @mouseleave="stopDrag"
    @mouseup="stopDrag"
  >
    <div
      ref="vueas-content"
      class="vueas-content"
    >
      <div class="vueas-slide">1</div>
      <div class="vueas-slide">2</div>
      <div class="vueas-slide">3</div>
      <div class="vueas-slide">4</div>
      <div class="vueas-slide">5</div>
    </div>

    <div
      v-if="controls"
      class="vueas-controls-wrapper"
    >
      <div
        class="vueas-left-control"
        @click="moveLeft"
        @mousedown="onLeftControlClicked"
        @touchstart="onLeftControlClicked"
        @mouseup="stopLongPress"
        @mouseout="stopLongPress"
        @touchend="stopLongPress"
        @touchleave="stopLongPress"
        @touchcancel="stopLongPress"
        @contextmenu.prevent
      >
        <slot name="left-control">
          <button v-text="'<<<'" />
        </slot>
      </div>
      <div
        class="vueas-right-control"
        @click="moveRight"
        @mousedown="onRightControlClicked"
        @touchstart="onRightControlClicked"
        @mouseup="stopLongPress"
        @mouseout="stopLongPress"
        @touchend="stopLongPress"
        @touchleave="stopLongPress"
        @touchcancel="stopLongPress"
        @contextmenu.prevent
      >
        <slot name="right-control">
          <button v-text="'>>>'" />
        </slot>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { TMovingKind } from '@/plugin/types/MovingKind';

@Component({})
export default class VueAwesomeSlider extends Vue {
  @Prop({ type: Boolean, default: true })
  controls!: boolean

  @Prop({ type: Number, default: 1 })
  showingSlidesCount!: number;

  translate = {
    step: { x: 20, y: 20 },
    position: { x: 0, y: 0 },
  }

  longPress = {
    active: false,
    timer: 0,
    interval: 50,
    delay: 300,
  }

  drag = {
    active: false,
    step: { x: 5, y: 5 },
  }

  touch = {
    pos: { x: 0, y: 0 },
    step: { x: 20, y: 20 },
  }

  get contentElement() {
    return this.$refs['vueas-content'] as HTMLElement;
  }

  mounted() {
    document.addEventListener('mousemove', this.onMouseMove);
    document.addEventListener('touchmove', this.onTouchMove);
  }

  private startDrag() {
    this.drag.active = true;
  }

  private stopDrag() {
    this.drag.active = false;
    this.resetTouchPositions();
  }

  private resetTouchPositions() {
    this.touch.pos.x = 0;
    this.touch.pos.y = 0;
  }

  private onTouchMove(e: TouchEvent) {
    const { pageX } = e.touches[0];

    if (!this.touch.pos.x) {
      this.touch.pos.x = pageX;
      return;
    }

    if (pageX > this.touch.pos.x) {
      this.moveRight('touch');
    } else if (pageX < this.touch.pos.x) {
      this.moveLeft('touch');
    }

    this.touch.pos.x = pageX;
  }

  private onMouseMove(e: MouseEvent) {
    if (this.drag.active) {
      if (e.movementX < 0) {
        this.moveLeft('drag');
      } else {
        this.moveRight('drag');
      }
    }
  }

  private onLeftControlClicked() {
    this.longPress.timer = setInterval(this.moveLeft, this.longPress.interval);
  }

  private onRightControlClicked() {
    this.longPress.timer = setInterval(this.moveRight, this.longPress.interval);
  }

  private stopLongPress() {
    clearInterval(this.longPress.timer);
  }

  private getStepValue(movingKind: TMovingKind): number {
    switch (movingKind) {
      case 'button':
        return this.translate.step.x;
      case 'drag':
        return this.drag.step.x;
      case 'touch':
        return this.touch.step.x;
      default:
        return 0;
    }
  }

  private moveLeft(movingKind: TMovingKind = 'button') {
    this.translate.position.x -= this.getStepValue(movingKind);
    this.setActualTranslate();
  }

  private moveRight(movingKind: TMovingKind = 'button') {
    this.translate.position.x += this.getStepValue(movingKind);
    this.setActualTranslate();
  }

  private setActualTranslate() {
    this.contentElement.style.transform = `translateX(${this.translate.position.x}px)`;
  }
}
</script>

<style lang="scss">
.vueas-wrapper {
  width: 100%;
  padding: 5px;
  border: 1px solid gray;
  overflow: hidden;
}

.vueas-content {
  display: flex;
  justify-content: flex-start;
}

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

.vueas-controls-wrapper {
  margin-top: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.vueas-left-control,
.vueas-right-control {
  flex-grow: 1;
  user-select: none;

  button {
    width: 100%;
  }
}
</style>
