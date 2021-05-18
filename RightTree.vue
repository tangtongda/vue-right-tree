<template>
  <div
    class="tree-right"
    v-if="showTree"
  >
    <div class="childs">
      <div
        class="child"
        v-for="(dataItem,index) in list"
        :key="dataItem.id +'-child-'+index"
      >
        <div
          class="child-item"
          :style="{marginRight: dataItem.children && dataItem.children.length > 1 ? '20px' :''}"
        >
          <div
            class="childname"
            :id="dataItem"
          >
            <div
              class="content-box"
              @click="traceDetail(dataItem)"
              :ref="dataItem.id"
              :id="dataItem.id"
            >
              <p
                v-for="(showItem,index3) in showfields"
                :key="'showItem'+index3"
              ><strong>{{showItem.name}}</strong>
                <span
                  class="data-font"
                  :style="{'color':showItem.color || showItem.colorEvent(dataItem[showItem.key])}"
                >
                  {{dataItem[showItem.key]}}
                </span>
              </p>
            </div>
            <div style="width:30px"></div>
            <div
              class="position-top"
              v-if="isFirst(dataItem.id) && domready"
              :style="position_top(dataItem.id,'top')"
            ></div>
            <div
              class="position-top"
              v-if="isLast(dataItem.id)"
              :style="position_top(dataItem.id,'bottom')"
            ></div>
          </div>
          <div style="width:160px"></div>
        </div>
        <!-- 递归组件展示子节点 -->
        <div
          class="child-children"
          v-if="dataItem.children && dataItem.children.length"
        >
          <RightTree
            :list="dataItem.children"
            :showfields="showfields"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LeaderLine from 'leader-line-vue'
export default {
    name: 'RightTree',
    components: {},
    data() {
        return {
            domready: false,
            lines: [],
        }
    },
    create(){
    },
    props: {
        list: {
            type: Array,
            default: () => [],
        },
        showfields: {
            type: Array,
            default: () => [],
        },
    },
    mounted() {
        this.$nextTick(() => {
            this.domready = true
            this.drawArrowLine()
        })
    },
    computed:{
        /**
         * 是否展示树计算属性
         */
        showTree(){
            return this.list && this.list.length
        },
    },
    beforeDestroy(){
        /**
         * 离开页面时销毁所有line
         */
        if(this.lines && this.lines.length){
            this.lines.forEach(line => {
                line.remove()
            })
        }
    },
    methods: {
        /**
         * 查看追踪详情
         */
        traceDetail(data) {
            this.$router.push(
                {
                    path: '/logTraceDetail',
                    query: {
                        id: data.id,
                        traceId: data.traceId,
                    },
                }
            )
        },
        /**
         * 递归绘制箭头
         */
        drawArrowLine(){
            this.drawLeaderLine(this.list)
        },
        /**
         * 销毁所有线条
         */
        destoryLine(){
            if(this.lines && this.lines.length){
                this.lines.forEach(line => {
                    line.remove()
                })
            }
        },
        /**
         * 根据上下级关系绘制线条
         */
        drawLeaderLine(list){
            list.forEach(element => {
                let start =  document.getElementById(element.id)
                if(element.children && element.children.length){
                    element.children.forEach(child => {
                        let line = LeaderLine.setLine(start,  document.getElementById(child.id))
                        line.startPlug = 'square'
                        line.color = '#1890ff'
                        line.path = 'grid'
                        line.size = 3
                        line.setOptions({
                            dash: {animation: true},
                        })
                        this.lines.push(line)
                    })
                    this.drawLeaderLine(element.children)
                }
            })
        },
        position_top(id, position) {
            let dom = document.getElementById(id)
            let height
            if (dom) {
                height = dom.clientHeight
            }
            let rt
            if (position === 'top') {
                rt = {
                    height: height / 2 - 2 + 'px',
                    top: 0,
                }
            }
            if (position === 'bottom') {
                rt = {
                    height: height / 2 + 1 + 'px',
                    bottom: 0,
                }
            }
            return rt
        },
        isFirst(id) {
            return (
                this.list.length > 1 && this.list.map((x) => x.id).indexOf(id) === 0
            )
        },
        isLast(id) {
            return (
                this.list.length > 1 &&
        this.list.map((x) => x.id).indexOf(id) === this.list.length - 1
            )
        },
    },
}
</script>

<style lang="less" scoped>
  .tree-right {
    p {
      margin: 0;
      font-size: 13px;
    }
    display: flex;
    .father {
      width: 70px;
      background-color: red;
      padding: 100px 10px;
    }
    .childs {
      .child {
        display: flex;
        background-color: #fff;
        .child-item {
          display: flex;
          align-items: center;
          margin: 10px 0;
          .childname {
            .content-box {
              text-align: left;
              border: 1px solid #e8e8e8;
              padding: 10px;
              border-radius: 6px;
              width: 100%;
              -webkit-box-shadow: 0 1px 5px 1px #dadce0;
              box-shadow: 0 1px 5px 1px #dadce0;
              font-family: 'Segoe UI', 'Roboto', arial, sans-serif;
              font-size: 13px;
              font-weight: 500;
            }
            cursor: pointer;
            height: 100%;
            display: flex;
            align-items: center;
            text-align: center;
            justify-content: center;
            position: relative;
            padding: 10px 0;
            .position-arrow {
              position: absolute;
              left: -22px;
            }
            .position-top {
              position: absolute;
              width: 3px;
              background-color: #fff;
              left: -23px;
              height: 10px;
            }
          }
          .childarrow {
            height: 100%;
            display: flex;
            align-items: center;
          }
        }
      }
      .child-children {
        display: flex;
        flex-direction: column;
        justify-content: center;
      }
    }
  }
  .data-font {
    font-weight: 600;
    font-family: 'Courier New', Courier, monospace;
  }
</style>