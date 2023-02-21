<template>
  <div ref="container" class="container" @scroll="scrollEvent($event)">    <!-- 可视区域的容器-->
    <div class="phantom" :style="{ height: listHeight + 'px' }"></div>        <!-- 为容器内的占位，高度为总列表高度，用于形成滚动条 -->
    <div class="list" :style="{ transform: getTransform }">
        <div class="list-item" 
            v-for="item of visibleData" 
            :key="item.id" 
            :style="{ height: itemSize + 'px',lineHeight: itemSize + 'px' }">
            {{ item.id }} - {{item.value}}
        </div>
    </div>    
  </div>
</template>

<script>
export default {
    name: 'myList',
    props: {
        // 所有列表数据
        listData:{
            type:Array,      // 类型
            default:() => []   // 默认值
        },
        // 每项列表高度
        itemSize: {
            type: Number,
            default:10
        }
    },
    
    computed:{
        // 列表总高度
        listHeight(){
            return this.listData.length * this.itemSize;
        },
        // 可显示的列表项数
        visibleCount(){
            return Math.ceil(this.screenHeight / this.itemSize)
        },
        // 偏移量对应的style
        getTransform(){
            return `translate3d(0,${this.startOffset}px,0)`;        // 滑轮滚动将value移动 translate3d 在3D空间内移动一个元素的位置   参数：横坐标、纵坐标、z坐标 移动纵坐标
        },
        // 获取真实显示列表数据
        visibleData(){
            return this.listData.slice(this.start, Math.min(this.end,this.listData.length));
        }
    },
    mounted() {
        this.screenHeight = this.$el.clientHeight;   // 获得屏幕高度
        this.start = 0;
        this.end = this.start + this.visibleCount;
    },
    data() {
        return {
            // 可视区域高度
            screenHeight:0,
            // 偏移量
            startOffset:0,
            // 起始索引
            start:0,
            // 结束索引
            end:null,
        };
    },
    methods: {
        scrollEvent() {
            // 当前滚动位置  获取打标识的ref 获取卷去的区域
            let scrollTop = this.$refs.container.scrollTop;
            // 此时的开始索引
            this.start = Math.floor(scrollTop / this.itemSize);
            // 此时的结束索引
            this.end = this.start + this.visibleCount;
            // 此时的偏移量
            this.startOffset = scrollTop - (scrollTop % this.itemSize);  // 鼠标滚动的偏移量 配合transform让列表看起来纵向移动
            console.log("监测到了滚动")
        }
    }

}
</script>

<style>
.container {
  height: 100%;
  overflow: auto;
  position: relative;
  -webkit-overflow-scrolling: touch;
}

.phantom {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}

.list {
  left: 0;
  right: 0;
  top: 0;
  position: absolute;
  text-align: center;
}

.list-item {
  padding: 10px;
  color: #555;
  box-sizing: border-box;
  /* background-color: antiquewhite; */
  border-bottom: 1px solid black;
}
</style>