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
      <div
        class="rectangle-node items-center justify-center row"
        ref="node3"
        style="top: 100px; left: 500px"
      >
        节点3
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node4"
        style="top: 100px; left: 700px"
      >
        节点4
      </div>

      <div
        class="rectangle-node items-center justify-center row"
        ref="node5"
        style="top: 300px; left: 100px"
      >
        节点5-source
      </div>
      <div
        class="rectangle-node items-center justify-center row"
        ref="node6"
        style="top: 300px; left: 300px"
      >
        节点6-target
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node7"
        style="top: 300px; left: 500px"
      >
        <span class="col">节点7-Bezier</span>
        <span class="col">curviness=150</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node8"
        style="top: 200px; left: 700px"
      >
        <span class="col">节点8-Bezier</span>
        <span class="col">curviness=150</span>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node9"
        style="top: 300px; left: 870px"
      >
        <span class="col">节点9-Bezier</span>
        <span class="col">curviness=10</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node10"
        style="top: 400px; left: 100px"
      >
        <span class="col">节点10-Straight</span>
        <span class="col">默认</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node11"
        style="top: 550px; left: 200px"
      >
        <span class="col">节点11-Straight</span>
        <span class="col">默认+stub=30</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node12"
        style="top: 400px; left: 300px"
      >
        <span class="col">节点12-Straight</span>
        <span class="col">stub=30,gap=20</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node13"
        style="top: 550px; left: 400px"
      >
        <span class="col">节点13-Straight</span>
        <span class="col">gap=20</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node14"
        style="top: 700px; left: 100px"
      >
        <span class="col">节点14-PaintStyle</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node15"
        style="top: 700px; left: 300px"
      >
        <span class="col">节点15-PaintStyle</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node16"
        style="top: 700px; left: 500px"
      >
        <span class="col">节点16-PaintStyle</span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node17"
        style="top: 700px; left: 700px"
      >
        <span class="col">节点17-PaintStyle</span>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node18"
        style="top: 400px; left: 500px"
      >
        <span class="col">节点18-Selector</span>
        <span class="dot source-selector"></span>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node19"
        style="top: 400px; left: 800px"
      >
        <span class="col">节点19-Selector</span>
        <span class="dot target-selector"></span>
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
const node18 = ref<Element>(Object());
const node19 = ref<Element>(Object());

const jsPlumb = ref<BrowserJsPlumbInstance>();

onMounted(() => {
  jsPlumb.value = newInstance({
    container: canvas.value,

    // 禁止断开连线配置
    // connectionsDetachable: false,

    // 自动重连配置
    // reattachConnections: true,

    // 全局线条样式配置
    // paintStyle: {
    //   stroke: 'red',
    //   strokeWidth: 5,
    //   outlineStroke: 'blue',
    //   outlineWidth: 5,
    // },
  });

  const endpoint3 = jsPlumb.value.addEndpoint(node3.value, {
    endpoint: {
      type: 'Rectangle',
      options: {
        width: 10,
        height: 10,
      },
    },
    anchor: AnchorLocations.Right,
    // connectionsDetachable: false,
    // reattachConnections: true,
  });
  const endpoint4 = jsPlumb.value.addEndpoint(node4.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: AnchorLocations.Left,
  });
  const endpoint5 = jsPlumb.value.addEndpoint(node5.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    source: true,
    anchor: AnchorLocations.Right,
  });

  const endpoint6 = jsPlumb.value.addEndpoint(node6.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    target: true,
    anchor: AnchorLocations.Left,
  });

  const endpoint7 = jsPlumb.value.addEndpoint(node7.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [1, 0.1, 1, 0],
  });
  const endpoint8 = jsPlumb.value.addEndpoint(node8.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 5,
      },
    },
    anchor: [0, 0.9, -1, 0],
  });

  jsPlumb.value.connect({
    source: node1.value,
    target: node2.value,
    connector: StraightConnector.type,
  });
  jsPlumb.value.connect({
    source: endpoint3,
    target: endpoint4,
    connector: 'Flowchart',
    // detachable: false,
    // reattach: true
  });
  jsPlumb.value.connect({
    source: endpoint7,
    target: endpoint8,
    connector: {
      type: 'Bezier',
      options: {
        curviness: 150,
      },
    },
  });
  jsPlumb.value.connect({
    source: node8.value,
    target: node9.value,
    endpoint: {
      type: 'Dot',
      options: {
        radius: 4,
      },
    },
    anchors: [
      [1, 0.5, 1, 0],
      [0, 0.5, -1, 0],
    ],
    connector: {
      type: 'Bezier',
      options: {
        curviness: 10,
      },
    },
  });
  jsPlumb.value.connect({
    source: node10.value,
    target: node11.value,
    endpoint: 'Dot',
    anchors: ['Bottom', [0.3, 0, 0, -1]],
    connector: 'Straight',
  });

  jsPlumb.value.connect({
    source: node11.value,
    target: node12.value,
    endpoint: 'Dot',
    anchors: [
      [0.7, 0, 0, -1],
      [0.3, 1, 0, 1],
    ],
    connector: {
      type: 'Straight',
      options: {
        stub: 30,
      },
    },
  });
  jsPlumb.value.connect({
    source: node12.value,
    target: node13.value,
    endpoint: 'Dot',
    anchors: [[0.7, 1, 0, 1], 'Top'],
    connector: {
      type: 'Straight',
      options: {
        gap: 20,
      },
    },
  });
  jsPlumb.value.connect({
    source: node14.value,
    target: node15.value,
    endpoint: 'Dot',
    anchors: ['Top', 'Top'],
    connector: 'Flowchart',
  });

  jsPlumb.value.connect({
    source: node16.value,
    target: node17.value,
    endpoint: 'Dot',
    anchors: ['Top', 'Top'],
    connector: 'Flowchart',
    paintStyle: {
      stroke: '#40BDEC',
      strokeWidth: 2,
      outlineStroke: '#FFF',
      outlineWidth: 8,
    },
    hoverPaintStyle: {
      stroke: '#40BDEC',
      strokeWidth: 4,
      outlineStroke: '#FFF',
      outlineWidth: 8,
    },
  });

  jsPlumb.value.manage(node18.value);
  jsPlumb.value.manage(node19.value);

  jsPlumb.value.addSourceSelector('.source-selector', {
    endpoint: 'Dot',
    anchor: 'Top',
    connector: 'Flowchart',
  });
  jsPlumb.value.addTargetSelector('.target-selector', {
    endpoint: 'Dot',
    anchor: 'Top',
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

  .dot {
    width: 20px;
    height: 20px;
    border-radius: 10px;
  }
  .source-selector {
    background-color: red;
  }
  .target-selector {
    background-color: blue;
  }
}
</style>
