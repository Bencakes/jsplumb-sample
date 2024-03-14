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
        节点2(坐标表示法)
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
        style="top: 300px; left: 300px"
      >
        节点4(坐标表示法)
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node5"
        style="top: 300px; left: 500px"
      >
        节点5-偏移
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node6"
        style="top: 300px; left: 700px"
      >
        节点6-Dynamic
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node7"
        style="top: 500px; left: 100px"
      >
        节点7-Dynamic
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node8"
        style="top: 500px; left: 300px"
      >
        节点8-Dynamic
      </div>

      <div
        class="circle-node items-center justify-center row"
        ref="node9"
        style="top: 500px; left: 500px"
      >
        节点9-Circle
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node10"
        style="top: 500px; left: 700px"
      >
        节点10-Rectange
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node11"
        style="top: 700px; left: 100px"
      >
        节点11-Continuous
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node12"
        style="top: 700px; left: 300px"
      >
        节点12-Continuous
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node13"
        style="top: 700px; left: 500px"
      >
        节点13-Continuous
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node14"
        style="top: 700px; left: 700px"
      >
        节点14-Continuous
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
const node3 = ref<Element>(Object());
const node4 = ref<Element>(Object());
const node5 = ref<Element>(Object());
const node6 = ref<Element>(Object());
const node7 = ref<Element>(Object());
const node8 = ref<Element>(Object());
const node9 = ref<Element>(Object());
const node10 = ref<Element>(Object());
const node11 = ref<Element>(Object());
const node12 = ref<Element>(Object());
const node13 = ref<Element>(Object());
const node14 = ref<Element>(Object());

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
    anchor: [0, 1, 0, 1],
  });
  const endpoint1_1 = jsPlumb.value.addEndpoint(node1.value, {
    endpoint: {
      type: 'Rectangle',
      options: {
        width: 10,
        height: 10,
      },
    },
    anchor: AnchorLocations.Top,
  });

  const endpoint2 = jsPlumb.value.addEndpoint(node2.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [0.5, 1, 0, 1], // Bottom
  });

  const endpoint3 = jsPlumb.value.addEndpoint(node3.value, {
    endpoint: {
      type: 'Rectangle',
      options: {
        width: 10,
        height: 10,
      },
    },
    anchor: AnchorLocations.Top,
  });
  const endpoint4 = jsPlumb.value.addEndpoint(node4.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [0.5, 1, 0, -1], // Bottom，朝上
  });
  const endpoint5 = jsPlumb.value.addEndpoint(node5.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [0.5, 1, 0, 1, 20, 10],
  });

  const endpoint6 = jsPlumb.value.addEndpoint(node6.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: 'AutoDefault',
  });

  const endpoint7 = jsPlumb.value.addEndpoint(node7.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [
      [0.2, 0, 0, -1],
      [1, 0.1, 1, 0],
      [1, 0.2, 1, 0],
      [1, 0.2, 1, 0],
      [1, 0.3, 1, 0],
      [1, 0.4, 1, 0],
      [1, 0.5, 1, 0],
      [1, 0.6, 1, 0],
      [1, 0.7, 1, 0],
      [1, 0.8, 1, 0],
      [1, 0.9, 1, 0],
      [0.8, 1, 0, 1],
      [0, 0.8, -1, 0],
    ],
  });
  const endpoint8 = jsPlumb.value.addEndpoint(node8.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [
      [0.2, 0, 0, -1],
      [1, 0.2, 1, 0],
      [0.8, 1, 0, 1],
      [0, 0.9, -1, 0],
      [0, 0.8, -1, 0],
      [0, 0.7, -1, 0],
      [0, 0.6, -1, 0],
      [0, 0.5, -1, 0],
      [0, 0.4, -1, 0],
      [0, 0.3, -1, 0],
      [0, 0.2, -1, 0],
      [0, 0.1, -1, 0],
    ],
  });
  const endpoint9 = jsPlumb.value.addEndpoint(node9.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: { type: 'Perimeter', options: { shape: 'Circle' } },
  });
  const endpoint10 = jsPlumb.value.addEndpoint(node10.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: { type: 'Perimeter', options: { shape: 'Rectangle' } },
  });

  const endpoint11 = jsPlumb.value.addEndpoint(node11.value, {
    anchor: 'Continuous',
    paintStyle: { fill: 'red' },
  });
  const endpoint12 = jsPlumb.value.addEndpoint(node12.value, {
    anchor: 'Continuous',
    paintStyle: { fill: 'blue' },
  });

  const endpoint13 = jsPlumb.value.addEndpoint(node13.value, {
    anchor: {
      type: 'Continuous',
      options: { faces: ['top', 'left'] },
    },
    paintStyle: { fill: 'red' },
  });
  const endpoint14 = jsPlumb.value.addEndpoint(node14.value, {
    anchor: { type: 'Continuous', options: { faces: ['bottom', 'right'] } },
    paintStyle: { fill: 'blue' },
  });

  jsPlumb.value.connect({
    source: endpoint1_1,
    target: endpoint2,
    connector: 'Flowchart',
  });
  jsPlumb.value.connect({
    source: endpoint3,
    target: endpoint4,
    connector: 'Flowchart',
  });
  jsPlumb.value.connect({
    source: endpoint5,
    target: endpoint6,
  });
  jsPlumb.value.connect({
    source: endpoint7,
    target: endpoint8,
  });
  jsPlumb.value.connect({
    source: endpoint9,
    target: endpoint10,
  });
  jsPlumb.value.connect({
    source: endpoint11,
    target: endpoint12,
  });
  jsPlumb.value.connect({
    source: endpoint13,
    target: endpoint14,
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
  .circle-node {
    position: absolute;
    border: 1px solid #000000;
    width: 80px;
    height: 80px;
    border-radius: 40px;
  }
}
</style>
