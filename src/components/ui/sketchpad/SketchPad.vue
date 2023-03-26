<script lang="ts" setup>
import { fabric } from 'fabric'

const CanvasWrapperRef = ref<HTMLCanvasElement>()
const SketchpadRef = ref<HTMLDivElement>()

const mouse = reactive({
  isDown: false,
  count: 0,
  from: {
    x: 0,
    y: 0
  },
  to: {
    x: 0,
    y: 0
  }
})

let fabricCanvas: fabric.Canvas | null = null

const setSize = (width = 0, height = 0) => {
  fabricCanvas?.setWidth(width || SketchpadRef.value?.clientWidth || 0)
  fabricCanvas?.setHeight(height || SketchpadRef.value?.clientHeight || 0)
}

const init = () => {
  fabricCanvas = new fabric.Canvas('canvas-wrapper', {
    isDrawingMode: true,
  })

  fabricCanvas.freeDrawingBrush.color = '#E34F51'
  fabricCanvas.freeDrawingBrush.width = 1.5

  setSize()

  fabricCanvas
    .on('mouse:down', e => {
      mouse.isDown = true

      mouse.from.x = e.pointer?.x || 0
      mouse.from.y = e.pointer?.y || 0
    })
    .on('mouse:move', e => {
      if (mouse.count % 2 && !mouse.isDown) return

      mouse.count++

      mouse.to.x = e.pointer?.x || 0
      mouse.to.y = e.pointer?.y || 0

      if (fabricCanvas) fabricCanvas.isDrawingMode = true
    })
    .on('mouse:up', e => {
      mouse.isDown = false

      mouse.to.x = e.pointer?.x || 0
      mouse.to.y = e.pointer?.y || 0
    })

}

onMounted(() => init())
</script>

<template>
  <div id="sketchpad" ref="SketchpadRef">
    <canvas id="canvas-wrapper" ref="CanvasWrapperRef" />
  </div>
</template>

<style lang="less" scoped>
#sketchpad {
  #canvas-wrapper {
    width: 100%;
    height: 100%;
  }
}
</style>