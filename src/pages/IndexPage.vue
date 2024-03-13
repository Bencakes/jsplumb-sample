<template>
  <q-page class="row items-center justify-evenly">
    <div class="canvas" ref="canvas">
      <div
        class="rectangle-node items-center justify-center row"
        ref="node1"
        style="top: 100px; left: 100px"
      >
        节点1
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node2"
        style="top: 100px; left: 300px"
      >
        节点2
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import {
  newInstance,
  BrowserJsPlumbInstance,
  AnchorLocations,
  DotEndpoint,
  RectangleEndpoint,
  BlankEndpoint,
} from '@jsplumb/browser-ui';

const canvas = ref<HTMLElement>();
const node1 = ref<Element>(Object());
const node2 = ref<Element>(Object());

const jsPlumb = ref<BrowserJsPlumbInstance>();

onMounted(() => {
  jsPlumb.value = newInstance({
    container: canvas.value,
  });

  const endpoint1 = jsPlumb.value.addEndpoint(node1.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 10,
      },
    },
    paintStyle: {
      // fill: 'red',
      stroke: 'blue',
      strokeWidth: 2,
      dashstyle: '6 2',
    },
  });
  jsPlumb.value.connect({
    source: endpoint1,
    target: node2.value,
    endpoint: 'Dot',
    anchor: 'Continuous',
    // anchor: {
    //   type: 'Perimeter',
    //   options: { shape: 'Rectangle' },
    // },
    // anchors: [
    //   { type: 'Perimeter', options: { shape: 'Triangle', rotation: 180 } },
    //   { type: 'Perimeter', options: { shape: 'Triangle', rotation: -180 } },
    // ],
  });
});
</script>

<style lang="scss">
.canvas {
  position: relative;
  width: 1000px;
  height: 800px;
  border: 1px solid #696868ba;

  .rectangle-node {
    position: absolute;
    width: 120px;
    height: 40px;
    border: 1px solid #000000;
  }

  // .circle-node {
  //   position: absolute;
  //   width: 80px;
  //   height: 80px;
  //   border: 1px solid #000000;
  //   border-radius: 40px;
  // }
}
</style>
