<template>
  <div id="container" ref="container"></div>
</template>

<script>
import G6 from "@antv/g6";
export default {
  name: "Dagre",
  props: {
    dagreData: {
      type: Object,
      default() {
        return {};
      }
    },
    dagreMount: {
      type: Function
    },
    graphConfig: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      const container = this.$refs.container || "container";
      G6.registerNode(
        "my-rect",
        {
          getAnchorPoints: function getAnchorPoints() {
            return [
              [0.5, 0],
              [0.5, 1]
            ];
          }
        },
        "rect"
      );

      const width = 500;
      const height = 800;

      const graph = new G6.Graph({
        container: container,
        width,
        height,
        fitCenter: true,
        layout: {
          type: "dagre",
          nodesepFunc: d => {
            if (d.id === "3") {
              return 500;
            }
            return 50;
          },
          ranksep: 70,
          controlPoints: true
        },
        modes: {
          default: [
            "drag-canvas",
            "zoom-canvas",
            "click-select",
            {
              offset: 30
            }
          ]
        },
        fitView: true,
        // modes: {
        //   default: ['drag-canvas'],
        // },
        defaultNode: {
          style: {
            fill: "#DEE9FF",
            stroke: "#5B8FF9"
          }
        },
        defaultEdge: {
          type: "cubic-vertical",
          style: {
            stroke: "#F6BD16",
            radius: 20,
            offset: 45,
            endArrow: true,
            lineWidth: 2
          }
        },
        ...this.graphConfig
      });
      graph.data(this.dagreData);
      graph.render();
      graph.on("node:click", e => {
        console.log(e);
        this.$emit("click", e);
      });
      // 抛出实例
      this.dagreMount(graph);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
h3
  margin 40px 0 0

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
