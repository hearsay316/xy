<template>
  <div id="app">
    <Dagre
      :dagreData="dagreData"
      :dagreMount="dagreMount"
      :graphConfig="graphConfig"
      @click="handleClickDagre"
    />
    <MessageBox :show.sync="MessageBoxShow" close="close">
      <div class="message">
        <h2 class="message-title">节点信息</h2>
        <div class="message-main">
          <h3 class="message-main-title">
            基本属性
          </h3>
          <ul class="message-list">
            <li class="message-list-item">
              <div class="message-list-item-left">节点名称 :</div>
              <div class="message-list-item-right">{{ info.label }}</div>
            </li>
            <li class="message-list-item" v-if="info.description">
              <div class="message-list-item-left">节点信息 :</div>
              <div class="message-list-item-right">{{ info.description }}</div>
            </li>
          </ul>
        </div>
      </div>
    </MessageBox>
  </div>
</template>

<script>
import Dagre from "./components/Dagre.vue";
import MessageBox from "./components/MessageBox.vue";
export default {
  name: "App",
  data() {
    return {
      MessageBoxShow: false, //弹框
      info: {
        //信息
        label: "",
        id: "",
        description: ""
      },
      // 节点数据
      dagreData: {
        nodes: [
          {
            id: "1",
            label: "开始",
            type: "my-rect"
          },
          {
            id: "2",
            type: "modelRect",
            label: "条件节点",
            description: "条件节点",
            linkPoints: {
              top: true,
              bottom: true,
              left: true,
              right: true,
              size: 10,
              fill: "#fff"
            },
            descriptionCfg: {
              style: {
                fill: "#000000",
                fontSize: 16
              }
            },
            preRect: {
              // 设置为 false，则不显示
              show: false
            },
            stateIcon: {
              show: false,
              img:
                "https://gw.alipayobjects.com/zos/basement_prod/c781088a-c635-452a-940c-0173663456d4.svg"
            },
            logoIcon: {
              // 是否显示 icon，值为 false 则不渲染 icon
              show: false
            }
          },
          {
            id: "3",
            type: "modelRect",
            description: "查询ES集群状态",
            label: "标准节点",
            stateIcon: {
              show: false,
              img:
                "https://gw.alipayobjects.com/zos/basement_prod/c781088a-c635-452a-940c-0173663456d4.svg"
            },
            logoIcon: {
              // 是否显示 icon，值为 false 则不渲染 icon
              show: false
            },
            descriptionCfg: {
              style: {
                fill: "#000000",
                fontSize: 16
              }
            }
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
          // 连接点
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
      },
      //graphConfig配置参数
      graphConfig: {}
    };
  },
  components: {
    Dagre,
    MessageBox
  },
  methods: {
    dagreMount(graph) {
      // 获取实例
      this.graph = graph;
      console.log(graph);
    },
    handleClickDagre(event) {
      // 获取元素的节点信息
      ["label", "id", "description"].forEach(item => {
        console.log(event.item._cfg.model[item]);
        if (event.item._cfg.model[item]) {
          this.info[item] = event.item._cfg.model[item];
        }
      });
      //打开弹框
      !this.MessageBoxShow ? (this.MessageBoxShow = true) : void 0;
    }
  }
};
</script>

<style lang="stylus">
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #2c3e50
  margin-top 60px
  .message{
    padding 15px
  }
  .message-title
    color #cccccc
  .message-main-title
    position relative
    margin-left   25px
  .message-main-title::before
    position absolute
    content:""
    left -10px
    top 0
    bottom 0
    margin auto
    background #002fff
    width 3px
    height:18px

  .message-list
    list-style none

    .message-list-item
      display flex
    .message-list-item-left
      margin-right 15px
</style>
