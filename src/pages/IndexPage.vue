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
        style="top: 100px; left: 500px"
      >
        节点2
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node3"
        style="top: 200px; left: 100px"
      >
        节点3
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node4"
        style="top: 200px; left: 400px"
      >
        节点4
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node5"
        style="top: 200px; left: 550px"
      >
        节点5
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node6"
        style="top: 200px; left: 800px"
      >
        节点6
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node7"
        style="top: 300px; left: 100px"
      >
        <span class="col">节点7-Overlay</span>
        <span class="col">Label</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node8"
        style="top: 300px; left: 400px"
      >
        <span class="col">节点8-Overlay</span>
        <span class="col">Label</span>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node9"
        style="top: 300px; left: 600px"
      >
        <span class="col">节点9-Overlay</span>
        <span class="col">Custom</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node10"
        style="top: 300px; left: 850px"
      >
        <span class="col">节点10-Overlay</span>
        <span class="col">Custom</span>
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
  StraightConnector,
  Component,
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
const node15 = ref<Element>(Object());
const node16 = ref<Element>(Object());
const node17 = ref<Element>(Object());

const jsPlumb = ref<BrowserJsPlumbInstance>();

onMounted(() => {
  jsPlumb.value = newInstance({
    container: canvas.value,
  });

  const endpoint3 = jsPlumb.value.addEndpoint(node3.value, {
    source: true,
    endpoint: 'Rectangle',
    anchor: AnchorLocations.Right,
    connectorOverlays: [
      { type: 'PlainArrow', options: { location: 1 } },
      { type: 'Label', options: { label: 'From Node3', id: 'node3-overlay' } },
    ],
  });
  const endpoint4 = jsPlumb.value.addEndpoint(node4.value, {
    target: true,
    endpoint: 'Dot',
    anchor: AnchorLocations.Left,
  });

  jsPlumb.value.connect({
    source: node1.value,
    target: node2.value,
    anchors: ['Right', 'Left'],
    connector: StraightConnector.type,
    overlays: [
      {
        type: 'Arrow',
        options: {
          location: 1,
        },
      },
      {
        type: 'PlainArrow',
        options: {
          location: 0.25,
        },
      },
      {
        type: 'Diamond',
        options: {
          location: 0.75,
        },
      },
      {
        type: 'Label',
        options: {
          label: 'myLabel',
          location: 0.5,
        },
      },
    ],
  });

  const conn_5_6 = jsPlumb.value.connect({
    source: node5.value,
    target: node6.value,
    anchors: ['Top', 'Top'],
    connector: 'Flowchart',
  });
  conn_5_6.addOverlay({
    type: 'PlainArrow',
    options: { location: 1 },
  });

  const conn_7_8 = jsPlumb.value.connect({
    source: node7.value,
    target: node8.value,
    connector: 'Flowchart',
    anchors: ['Top', 'Top'],
    overlays: [
      {
        type: 'Label',
        options: {
          location: 0.3,
          label: 'Label1',
          cssClass: 'label-e1',
          id: 'lab1',
        },
      },
      {
        type: 'Label',
        options: {
          location: 0.6,
          label: 'Label2',
          labelStyle: { color: 'blue' },
          id: 'lab2',
        },
      },
    ],
  });

  const conn_7_8_1 = jsPlumb.value.connect({
    source: node7.value,
    target: node8.value,
    connector: 'Flowchart',
    anchors: ['Bottom', 'Bottom'],
    label: 'label by connect',
  });

  jsPlumb.value.connect({
    source: node9.value,
    target: node10.value,
    anchors: ['Top', 'Top'],
    connector: 'Flowchart',
    overlays: [
      { type: 'Arrow', options: { location: 1 } },
      {
        type: 'Custom',
        options: {
          create: (component: Component) => {
            console.log(component);
            const d = document.createElement('span');
            d.setAttribute('class', 'line-btn');
            d.addEventListener('click', (event: MouseEvent) => {
              event.stopPropagation();
              alert('btn click');
            });
            d.innerHTML = '按钮';
            return d;
          },
          location: 0.5,
        },
      },
    ],
  });

  // 隐藏所有overlay
  conn_7_8.hideOverlays();
  // 显示所有overlay
  conn_7_8.showOverlays();

  // 通过线条实例来显示/隐藏overlay
  conn_7_8.hideOverlay('lab1');
  conn_7_8.showOverlay('lab1');

  // 通过overlay实例来显示/隐藏label
  const lab2Overlay = conn_7_8.getOverlay('lab2');
  lab2Overlay.setVisible(false);
  lab2Overlay.setVisible(true);
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

  .dot {
    width: 16px;
    height: 16px;
    border-radius: 8px;
  }
  .red {
    background: red;
  }
  .blue {
    background: blue;
  }

  .label-e1 {
    color: red;
    padding: 4px;
    background-color: blue;
    height: 40px;
  }

  .line-btn {
    color: white;
    background-color: #40bdec;
    font-size: 12px;
    border-radius: 4px;
    padding: 3px;
    cursor: pointer;
  }
}
</style>
