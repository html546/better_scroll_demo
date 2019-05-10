<template>
  <div id="app">
    <div class="head">
      head
    </div>
    <div class="content">
      <div class="left" ref="left">
        <ul>
          <li v-for="(item,index) in left" :key="index" :class="{current:currentIndex == index}" @click="selectItem(index,$event)">
            <span class="left-item">{{item}}</span>
          </li>
        </ul>
      </div>
      <div class="right" ref="right">
        <ul>
          <li v-for="item in right" class="right-item right-item-hook">
            <h2>{{item.name}}</h2>
            <ul>
              <li v-for="num in item.content">
                <div>{{item.name+num}}</div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
    <div class="foot">foot</div>
  </div>
</template>

<script>
import BetterScroll from 'better-scroll'
export default {
  name: 'BetterScroll',
  data() {
    return {
      left: ['数学', '语文', '英语', '物理', '化学', '其他'],
      right: [
        {
          name: '数学',
          content: ['1', '2', '3', '4', '5', '6', '7']
        },
        {
          name: '语文',
          content: ['3', '4', '5', '6']
        },
        {
          name: '英语',
          content: ['1', '2', '3', '4', '5', '6', '7']
        },
        {
          name: '物理',
          content: ['5', '6', '7']
        },
        {
          name: '化学',
          content: ['4', '5', '6', '7']
        },
        {
          name: '其他',
          content: ['3', '4', '5', '6', '7']
        }
      ],
      listHeight: [],
      scrollY: 0,
      clickEvent: false
    }
  },
  computed: {
    currentIndex() {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        if (!height2 || (this.scrollY >= height && this.scrollY < height2)) {
          return i;
        }
        if (this.listHeight[this.listHeight.length - 1] - this.$refs.right.clientHeight <= this.scrollY) {
          if (this.clickTrue) {
            return this.currentNum;
          } else {
            return (this.listHeight.length - 1)
          }
        }
      }
      return 0;
      /* if (this.listHeight[this.listHeight.length - 1] - this.$refs.right.innerHeight <= this.scrollY) {
        return this.currentNum
      } */
    }
  },
  mounted() {
    this.$nextTick(() => {
      this._initScroll();
      this._getHeight();
    })
  },
  methods: {
    _initScroll() {
      this.lefts = new BetterScroll(this.$refs.left, {
        click: true
      })
      this.rights = new BetterScroll(this.$refs.right, {
        probeType: 3
      })
      this.rights.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
    _getHeight() {
      let rightItems = this.$refs.right.getElementsByClassName('right-item-hook');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < rightItems.length; i++) {
        let item = rightItems[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    },
    selectItem(index, event) {
      this.currentNum = index;
      this.clickTrue = true;
      if (!event._constructed) {
        return;
      } else {
        let rightItems = this.$refs.right.getElementsByClassName('right-item-hook');
        let el = rightItems[index];
        this.rights.scrollToElement(el, 300)
      }
    }
  },
}
</script>


<style lang="less">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.content {
  display: flex;
  position: absolute;
  top: 100px;
  bottom: 100px;
  width: 100%;
  overflow: hidden;
  background: #eee;
}
.left {
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}
.left li {
  width: 100%;
  height: 100%;
}
.current {
  background: red;
}
.left-item {
  display: block;
  width: 100%;
  height: 100px;
  line-height: 50px;
  text-align: center;
  border-bottom: 1px solid yellow;
}
.right {
  flex: 1;
}
.right-item li {
  width: 100%;
  height: 100px;
  line-height: 100px;
  text-align: center;
  border-bottom: 1px solid yellow;
}
</style>
