<template>
  <div id="container"></div>
</template>

<script>
import G6 from "@antv/g6";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
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
      const data = {
        nodes: [
          {
            id: "1",
            label: "开始",
            type: "my-rect"
          },
          {
            id: "2",
            type: "my-rect",
            label: "条件节点"
          },
          {
            id: "3",
            type: "my-rect",
            label: "查询ES集群状态"
          },
          {
            id: "4",
            x: 900,
            y: 100,
            type: "my-rect",
            label: "结束"
          }
        ],
        edges: [
          {
            source: "1",
            target: "2"
          },
          {
            source: "1",
            target: "3"
          },
          {
            source: "2",
            target: "3"
          },
          {
            source: "3",
            target: "4"
          },
          {
            source: "1",
            target: "4"
          }
        ]
      };

      G6.registerNode(
        "sql",
        {
          drawShape(cfg, group) {
            const rect = group.addShape("rect", {
              attrs: {
                x: -75,
                y: -25,
                width: 150,
                height: 50,
                radius: 10,
                stroke: "#5B8FF9",
                fill: "#C6E5FF",
                lineWidth: 3
              },
              name: "rect-shape"
            });
            if (cfg.name) {
              group.addShape("text", {
                attrs: {
                  text: cfg.name,
                  x: 0,
                  y: 0,
                  fill: "#00287E",
                  fontSize: 14,
                  textAlign: "center",
                  textBaseline: "middle",
                  fontWeight: "bold"
                },
                name: "text-shape"
              });
            }
            return rect;
          }
        },
        "single-node"
      );

      const width = document.getElementById("container").scrollWidth;
      const height = document.getElementById("container").scrollHeight || 800;
      const graph = new G6.Graph({
        container: "container",
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
        // defaultNode: {
        //   type: "sql"
        // },
        // defaultEdge: {
        //   type: "polyline",
        //   style: {
        //     radius: 20,
        //     offset: 45,
        //     endArrow: true,
        //     lineWidth: 2,
        //     stroke: "#C2C8D5"
        //   }
        // },
        nodeStateStyles: {
          selected: {
            stroke: "#d9d9d9",
            fill: "#5394ef"
          }
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
        }
      });
      graph.data(data);
      graph.render();
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
