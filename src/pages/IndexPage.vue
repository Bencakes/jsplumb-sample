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
        style="top: 100px; left: 400px"
      >
        节点2
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node3"
        style="top: 300px; left: 100px"
      >
        节点3
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node4"
        style="top: 300px; left: 400px"
      >
        节点4
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
} from '@jsplumb/browser-ui';

const canvas = ref<HTMLElement>();
const node1 = ref<Element>(Object());
const node2 = ref<Element>(Object());
const node3 = ref<Element>(Object());
const node4 = ref<Element>(Object());

const jsPlumb = ref<BrowserJsPlumbInstance>();

onMounted(() => {
  jsPlumb.value = newInstance({
    container: canvas.value,
  });

  const endpoint3 = jsPlumb.value.addEndpoint(node3.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 4,
      },
    },
    anchor: [AnchorLocations.Right],
  });

  const endpoint4 = jsPlumb.value.addEndpoint(node4.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 4,
      },
    },
    anchor: [AnchorLocations.Left],
  });

  jsPlumb.value.connect({
    source: node1.value,
    target: node2.value,
    connector: 'Flowchart',
  });

  jsPlumb.value.connect({
    source: endpoint3,
    target: endpoint4,
    connector: 'Flowchart',
    overlays: [{ type: 'Arrow', options: { location: 1 } }],
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
    width: 100px;
    height: 40px;
    border: 1px solid #000000;
  }
}
</style>
