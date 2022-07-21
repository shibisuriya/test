<template>
  <div
    class="grid-stack-item"
    :gs-id="id"
    :gs-x="x"
    :gs-y="y"
    :gs-w="w"
    :gs-h="h"
    :gs-min-w="minW"
    :gs-min-h="minH"
    :gs-max-w="maxW"
    :gs-max-h="maxH"
    :gs-no-resize="resize"
    :gs-no-move="move"
    :gs-locked="locked"
  >
    <div :style="showScroll" class="grid-stack-item-content">
      <slot></slot>
    </div>
  </div>
</template>
<script>
export default {
  inject: ['eventBus', 'layout', 'sublayout'],
  props: {
    id: {
      type: String,
      required: true,
    },
    x: {
      type: Number,
      required: true,
    },
    y: {
      type: Number,
      required: true,
    },
    w: {
      type: Number,
      required: true,
    },
    h: {
      type: Number,
      required: true,
    },
    minW: {
      type: Number,
      default: 0,
    },
    minH: {
      type: Number,
      default: 0,
    },
    maxW: {
      type: Number,
      default: 0,
    },
    maxH: {
      type: Number,
      default: 0,
    },
    resize: {
      type: Boolean,
      default: false,
    },
    locked: {
      type: Boolean,
      default: false,
    },
    move: {
      type: Boolean,
      default: false,
    },
    resizeHandles: {
      type: String,
      default: null,
    },
    noScroll: {
      // Show scroll on gridItem or not...
      type: Boolean,
      default: false,
    },
  },
  computed: {
    showScroll() {
      return {
        overflow: this.noScroll ? 'hidden' : 'auto',
      }
    },
    position() {
      return `${this.x}-${this.y}-${this.w}-${this.h}`
    },
  },
  watch: {
    position: {
      handler: function (old, newVal) {
        newVal
        console.log('removing: ', this.$el)
        const obj = {
          x: this.x,
          y: this.y,
          w: this.w,
          h: this.h,
        }
        if (this.sublayout) {
          this.sublayout.grid.update(this.$el, obj)
        } else {
          this.layout.grid.update(this.$el, obj)
        }
      },
    },
  },
  mounted() {
      console.log('added: ', this.$el, this.layout.grid, this.sublayout)
    if (this.sublayout) {
      this.sublayout.grid.makeWidget(this.$el)
    } else if (this.layout.grid) {
      this.layout.grid.makeWidget(this.$el)
    }
  },

  beforeDestroy() {
    console.log('removing: ', this.$el)
    if (this.sublayout) {
      this.sublayout.grid.removeWidget(this.$el, false, false)
    } else {
      this.layout.grid.removeWidget(this.$el, false, false)
    }
  },
}
</script>
