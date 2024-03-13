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
        class="rectangle-node items-center justify-center column"
        ref="node4"
        style="top: 300px; left: 100px"
      >
        <div class="col">节点4-Dot</div>
        <div class="col">radius=3</div>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node5"
        style="top: 300px; left: 300px"
      >
        <div class="col">节点5-Dot</div>
        <div class="col">radius=10</div>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node6"
        style="top: 300px; left: 500px"
      >
        <div class="col">节点6-Rectangle</div>
        <div class="col">w:h=20:10</div>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node7"
        style="top: 500px; left: 100px"
      >
        <div class="col">节点7-Dot</div>
        <div class="col">cssClass</div>
      </div>
      <div
        class="rectangle-node items-center justify-center column"
        ref="node8"
        style="top: 500px; left: 300px"
      >
        <div class="col">节点8-Rectangle</div>
        <div class="col">cssClass</div>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node9"
        style="top: 500px; left: 500px"
      >
        <div class="col">节点9-Dot</div>
        <div class="col">hoverClass</div>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node10"
        style="top: 500px; left: 700px"
      >
        <div class="col">节点10-Dot</div>
        <div class="col">cssClass</div>
      </div>

      <div
        class="rectangle-node items-center justify-center column"
        ref="node11"
        style="top: 700px; left: 100px"
      >
        <div class="col">节点11-Dot</div>
        <div class="col">调整样式</div>
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

const jsPlumb = ref<BrowserJsPlumbInstance>();

onMounted(() => {
  jsPlumb.value = newInstance({
    container: canvas.value,

    // 设置全局的endpoint样式
    // endpointStyle: {
    //   fill: 'red',
    // },
  });

  // 创建Endpoint的方式一
  // 使用字符串'Dot'指定Endpoint类型，字符串为大小写敏感
  // 默认情况下的Dot类型的radius=5px

  const endpoint1 = jsPlumb.value.addEndpoint(node1.value, {
    endpoint: 'Dot',
    source: true,
  });

  // 创建Endpoint的方式二
  // 默认情况下的Dot类型的radius=5px

  const endpoint2 = jsPlumb.value.addEndpoint(node2.value, {
    endpoint: RectangleEndpoint.type,
    target: true,
  });

  // 创建Endpoint的方式三
  // 传入一个对像，可以对Endpoint实现更加细腻的控制
  const endpoint3 = jsPlumb.value.addEndpoint(node3.value, {
    endpoint: {
      type: 'Blank',
      options: {},
    },
  });

  const endpoint4 = jsPlumb.value.addEndpoint(node4.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 3,
      },
    },
  });
  const endpoint5 = jsPlumb.value.addEndpoint(node5.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 10,
      },
    },
  });
  const endpoint6 = jsPlumb.value.addEndpoint(node6.value, {
    endpoint: {
      type: 'Rectangle',
      options: {
        width: 20,
        height: 10,
      },
    },
  });

  const endpoint7 = jsPlumb.value.addEndpoint(node7.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 10,
        cssClass: 'endpoint-css',
      },
    },
  });
  const endpoint8 = jsPlumb.value.addEndpoint(node8.value, {
    endpoint: {
      type: 'Rectangle',
      options: {
        width: 20,
        height: 10,
        cssClass: 'endpoint-css',
      },
    },
  });

  // 不会触发hover
  const endpoint9 = jsPlumb.value.addEndpoint(node9.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 12,
        hoverClass: 'endpoint-css',
      },
    },
  });

  const endpoint10 = jsPlumb.value.addEndpoint(node10.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 10,
        cssClass: 'endpoint-margin',
      },
    },
  });

  const endpoint11 = jsPlumb.value.addEndpoint(node11.value, {
    endpoint: {
      type: 'Dot',
      options: {
        radius: 6,
        cssClass: 'real-css',
      },
    },
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
}

.endpoint-css {
  background-color: red;
  border: 2px solid blue;
}

.endpoint-margin {
  margin-top: 10px;
}

svg.real-css circle {
  fill: red;
  stroke: blue;
  stroke-width: 2px;
}
</style>
