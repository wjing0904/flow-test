<template>
  <div class="flow-chart" @keydown="stopTab">
    <div ref="container" class="container"></div>
  </div>
</template>

<script lang="ts">
  import { ref } from 'vue';
  import LogicFlow from '@logicflow/core';
  import FlowPanel from './FlowPanel/index';
  export default {
    setup() {
      const count = ref(0);
      const currentNode = ref(null);
      return {
        count,
        currentNode,
      };
    },
    mounted() {
      this.lf = new LogicFlow({
        container: this.$refs.container,
        height: 1000,
        grid: false,
        edgeTextEdit: false,
        // adjustEdge: false,
        adjustNodePosition: false,
        hoverOutline: false,
        edgeSelectedOutline: false,
        stopScrollGraph: true,
        stopZoomGraph: true,
        keyboard: {
          enabled: true,
        },
        // keyboard: true,
        plugins: [FlowPanel],
        stopPropagation: ['edge:click'], // 阻止传递边的点击事件
      });
      this.lf.render({
        nodes: [
          {
            id: '1',
            x: 200,
            y: 200,
            type: 'start-node',
            text: '开始',
            properties: { nodeType: 1 },
          },
          {
            id: '3',
            x: 200,
            y: 600,
            type: 'end-node',
            text: '结束',
            properties: { nodeType: 3 },
          },
        ],
        edges: [
          {
            sourceNodeId: '1',
            targetNodeId: '3',
            type: 'node-edge',
          },
        ],
      });
    },
    methods: {
      changeStyle(style) {
        this.lf.setProperties(this.currentNode.id, {
          style,
        });
      },
      stopTab(ev) {
        if (ev.code === 'Tab') {
          ev.preventDefault();
        }
      },
    },
  };
</script>

<style scoped>
  .container {
    width: 100%;
    height: 100%;
  }
  .flow-chart {
    position: relative;
    width: 100%;
    height: 100%;
  }
  .flow-chart :deep(.lf-graph) {
    background: rgb(247, 247, 247);
  }
  .flow-chart :deep(.lf-red-node),
  .flow-chart :deep(.lf-element-text) {
    cursor: move;
  }
  .flow-chart :deep(svg) {
    display: block;
  }
  .flow-chart-palette {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1;
  }
  .setting-panel {
    position: absolute;
    top: 0;
    right: 0;
  }
</style>
