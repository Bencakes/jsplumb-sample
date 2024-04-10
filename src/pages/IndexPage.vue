<template>
  <q-page class="row items-center justify-evenly">
    <div class="body">
      <div
        class="toolbar row items-center justify-start"
        @mousemove="dragMove"
        @mouseup="dragNodeFinish"
      >
        <div
          class="rectangle element"
          @mousedown="dragStart('rectangle')"
        ></div>
        <div class="prism element" @mousedown="dragStart('prism')"></div>
        <div class="circle element" @mousedown="dragStart('circle')"></div>
        <div
          class="toolbar-drag-node"
          :class="dragNodeType"
          :style="{
            top: dragNodePosition.y + 'px',
            left: dragNodePosition.x + 'px',
          }"
          v-if="dragStartFlag === true && draggingFlag == true"
        ></div>
      </div>
      <div
        class="canvas"
        ref="canvas"
        @mouseup="dragNodeFinish"
        @mousemove="nodeMove"
        @click="clickCanvas"
      >
        <div
          v-for="n in nodeList"
          :key="n"
          :class="n.type + '-node'"
          :style="{ top: n.y + 'px', left: n.x + 'px' }"
          ref="nodeListRefs"
        ></div>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import {
  newInstance,
  BrowserJsPlumbInstance,
  AnchorLocations,
  ContainmentType,
  Connection,
  Component,
  EVENT_CONNECTION_CLICK,
  EVENT_CONNECTION_DBL_CLICK,
  EVENT_CONNECTION,
  ConnectionEstablishedParams,
  LabelOverlay,
} from '@jsplumb/browser-ui';

const canvas = ref<HTMLElement>();

const dragStartFlag = ref(false);
const draggingFlag = ref(false);

const dragNodeType = ref('');
const dragNodePosition = ref({
  x: 0,
  y: 0,
});

const nodeList = ref<any[]>([]);
// 画布中的节点列表DOM引用
const nodeListRefs = ref<any[]>([]);

const jsPlumb = ref<BrowserJsPlumbInstance>();

onMounted(() => {
  jsPlumb.value = newInstance({
    container: canvas.value,
    paintStyle: {
      stroke: '#40BDEC',
      strokeWidth: 2,
      outlineStroke: '#ffffff00',
      outlineWidth: 6,
    },
    hoverPaintStyle: {
      stroke: '#40BDEC',
      strokeWidth: 3,
      outlineStroke: '#ffffff00',
      outlineWidth: 6,
    },
    endpointStyle: { stroke: '#ccc' },
    connector: {
      type: 'Flowchart',
      options: {
        cssClass: 'line',
        hoverClass: 'line',
      },
    },
    connectionOverlays: [
      {
        type: 'Arrow',
        options: { location: 1, width: 10, length: 8 },
      },
    ],
    dragOptions: {
      containment: ContainmentType.parentEnclosed,
      grid: { w: 20, h: 20 },
    },
  });

  jsPlumb.value.bind(EVENT_CONNECTION_CLICK, (connection: Connection) => {
    _connectionClick(connection);
  });

  jsPlumb.value.bind(
    EVENT_CONNECTION,
    (params: ConnectionEstablishedParams) => {
      // 给连线在起始位置添加一个可以用于删除连线的按钮
      const cancelOverlayId = params.connection.getId() + 'cancelOverlay';
      params.connection.addOverlay({
        type: 'Custom',
        options: {
          create: () => {
            const d = document.createElement('span');
            d.setAttribute('class', 'line-cancel-icon hide');
            d.onclick = () => removeConnection(params.connection);
            d.innerHTML = 'x';
            return d;
          },
          location: 0,
          id: cancelOverlayId,
        },
      });

      // 给连线在中间添加一个输入框
      const inputOverlayId = params.connection.getId() + 'inputOverlay';
      params.connection.addOverlay({
        type: 'Custom',
        options: {
          create: () => {
            const d = document.createElement('input');
            d.setAttribute('class', 'input-overlay hide');
            d.setAttribute(
              'id',
              'conn-input-overlay-' + params.connection.getId()
            );
            d.onclick = (event) => clickInputOverlay(event);
            return d;
          },
          location: 0.5,
          id: inputOverlayId,
        },
      });
    }
  );

  jsPlumb.value.bind(EVENT_CONNECTION_DBL_CLICK, (connector: Connection) => {
    console.log(connector, connector.getLabel(), connector.getLabelOverlay());
    const overlayId = 'conn-input-overlay-' + connector.getId();
    const overlay = connector.getOverlay(overlayId) as LabelOverlay;
    if (overlay && overlay.isVisible()) {
      const input = document.getElementById(overlayId) as HTMLInputElement;
      const label = overlay.getLabel();
      connector.removeOverlay(overlayId);
      input.value = label;
    }
    connector.showOverlay(connector.getId() + 'inputOverlay');
  });
});

function _connectionClick(connector: Connection) {
  connector.showOverlay(connector.getId() + 'cancelOverlay');
}

// mousedown事件，在工具栏中开始拖拽图标
function dragStart(nodeType: string) {
  dragStartFlag.value = true;
  dragNodeType.value = nodeType;
}
// mousemove事件，工具栏中的结点移动
function dragMove(e: MouseEvent) {
  if (!dragStartFlag.value) {
    return;
  }

  if (dragStartFlag.value && !draggingFlag.value) {
    dragNodePosition.value.x = e.clientX;
    dragNodePosition.value.y = e.clientY;
    draggingFlag.value = true;
  } else if (draggingFlag.value) {
    dragNodePosition.value.x = e.clientX;
    dragNodePosition.value.y = e.clientY;
  }
}

// 画布中的结点移动
function nodeMove(e: MouseEvent) {
  if (!dragNodePosition.value || !canvas.value) {
    return;
  }

  if (draggingFlag.value) {
    // 表示节点是从工具栏拖拽到画布中的，此时拖拽的节点还属于工具栏的子节点而不是在画布中
    dragNodePosition.value.x = e.clientX;
    dragNodePosition.value.y = e.clientY;
  }
}

// mouseup事件，工具栏与画布都绑定了此事件
function dragNodeFinish(e: MouseEvent) {
  if (!canvas.value) {
    dragStartFlag.value = false;
    draggingFlag.value = false;
    return;
  }

  if (draggingFlag.value) {
    // 表示是从工具栏开始的，正在进行中的拖拽事件
    dragStartFlag.value = false;
    draggingFlag.value = false;
    // 判断当前鼠标位置，是否在画布内，如果在画布内，则创建节点
    let canvasRect = canvas.value.getBoundingClientRect();
    console.log(
      'clientX:' + e.clientX + ' -- ' + canvasRect.x,
      'clientY:' + e.clientY + ' -- ' + canvasRect.y
    );
    if (e.clientX > canvasRect.x && e.clientY > canvasRect.y) {
      _createNode();
    }
  }
}

function _createNode() {
  if (!dragNodePosition.value || !canvas.value) {
    return;
  }

  const newNode = {
    type: dragNodeType.value,
    x: dragNodePosition.value.x - canvas.value.getBoundingClientRect().left,
    y: dragNodePosition.value.y - canvas.value.getBoundingClientRect().top,
    refs: null,
  };

  nodeList.value.push(newNode);

  nextTick().then(() => {
    newNode.refs = nodeListRefs.value[nodeList.value.length - 1];
    initialJsPlumbNode(newNode);
  });
}

function initialJsPlumbNode(node: any) {
  if (!node.refs || !jsPlumb.value) {
    return;
  }

  jsPlumb.value.manage(node.refs);
  const anchors: AnchorLocations[] = [
    AnchorLocations.Top,
    AnchorLocations.Bottom,
    AnchorLocations.Left,
    AnchorLocations.Right,
    AnchorLocations.TopLeft,
    AnchorLocations.TopRight,
    AnchorLocations.BottomLeft,
    AnchorLocations.BottomRight,
  ];
  for (const anchorLocation of anchors) {
    const ep = jsPlumb.value.addEndpoint(node.refs, {
      endpoint: {
        type: 'Dot',
        options: {
          radius: 4,
        },
      },
      source: true,
      target: true,
      anchor: anchorLocation,
    });
  }
}

function removeConnection(conn: Connection) {
  jsPlumb.value?.deleteConnection(conn);
}

function clickInputOverlay(event: MouseEvent) {
  event.stopPropagation();
}

function clickCanvas() {
  jsPlumb.value?.select().each((connection) => {
    const overlayId = connection.getId() + 'cancelOverlay';
    connection.hideOverlay(overlayId);
    const inputId = connection.getId() + 'inputOverlay';
    const inputOverlay = connection.getOverlay(inputId);

    if (inputOverlay && inputOverlay.isVisible()) {
      connection.hideOverlay(inputId);
      const label = (
        document.getElementById(
          'conn-input-overlay-' + connection.getId()
        ) as HTMLInputElement
      ).value;
      if (label && label.trim()) {
        const labelOverlay = connection.addOverlay({
          type: 'Label',
          options: {
            label: label,
            location: 0.5,
            id: 'conn-input-overlay-' + connection.getId(),
          },
        });
        jsPlumb.value?.setHover(connection, false);
      }
    }
  });
}
</script>

<style lang="scss">
.toolbar {
  // position: relative;
  width: 1000px;
  height: 80px;
  margin-top: 10px;
  border: 1px solid #696868ba;
  padding-left: 20px;

  .toolbar-drag-node {
    position: absolute;
  }

  .element {
    margin-right: 40px;
  }

  .rectangle {
    width: 100px;
    height: 40px;
    border: 1px solid #000000;
  }
  .circle {
    border: 1px solid #000000;
    width: 60px;
    height: 60px;
    border-radius: 40px;
  }
  .prism {
    width: 60px;
    height: 60px;
    border: 1px solid #000000;
    transform: rotate(45deg) skew(-12deg, -12deg);
  }
}
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
  .prism-node {
    position: absolute;
    width: 70px;
    height: 70px;
    border: 1px solid #000000;
    transform: rotate(45deg) skew(-12deg, -12deg);
  }

  .line {
    cursor: pointer;
  }
  .line-cancel-icon {
    text-align: center;
    width: 12px;
    height: 12px;
    line-height: 12px;
    font-size: 12px;
    font-weight: bolder;
    color: white;
    background-color: red;
    border-radius: 6px;
    cursor: pointer;
  }
  .line-cancel-icon.hide {
    display: none;
  }
  .input-overlay {
    width: 80px;
  }
  .input-overlay.hide {
    display: none;
  }
}
</style>
