<template>
  <div class="wrap">
    <div class="screen" ref="scr">
      <div class="swiper-group" ref="sli" @mouseenter="stop" @mouseleave="start">
        <ul v-for="(list,index) in srcList" :key="index">
          <li>
            <img :src="list.img1" />
          </li>
          <li>
            <img :src="list.img2" />
          </li>
        </ul>
      </div>
    </div>
    <button type="button" class="left" @click="prev">&lt;</button>
    <button type="button" class="right" @click="next">&gt;</button>
  </div>
</template>

<script>
export default {
  name: "SwiperTest",
  data() {
    return {
      srcList: [
        {
          img1: require("@/assets/images/five.jpg"),
          img2: require("@/assets/images/six.jpg")
        },
        {
          img1: require("@/assets/images/one.jpg"),
          img2: require("@/assets/images/two.jpg")
        },
        {
          img1: require("@/assets/images/three.jpg"),
          img2: require("@/assets/images/four.jpg")
        },
        {
          img1: require("@/assets/images/five.jpg"),
          img2: require("@/assets/images/six.jpg")
        },
        {
          img1: require("@/assets/images/one.jpg"),
          img2: require("@/assets/images/two.jpg")
        }
      ],
      index: 2,
      timer: null, //定时器
      screenWidth:0
    };
  },
  methods: {
    //自动播放
    autoLoop() {
      this.timer = setInterval(() => {
        this.move();
      }, 3000);
    },
    //移动函数
    move() {
      this.$refs.sli.style.left = -this.screenWidth * this.index + "px";
      this.$refs.sli.style.transition = "all 0.5s";
      this.index++;
      if (this.index > this.srcList.length - 1) {
        setTimeout(() => {
          this.$refs.sli.style.left = -this.screenWidth + "px";
          this.$refs.sli.style.transition = "all 0s";
          this.index = 2;
        }, 500);
      }
    },
    //鼠标移入
    stop() {
      clearInterval(this.timer);
    },
    //鼠标移出
    start() {
      this.autoLoop();
    },
    //下一页
    next() {  
      clearInterval(this.timer);
      this.move();
      this.autoLoop();
    },
    //上一页
    prev() {
      clearInterval(this.timer);
      this.$refs.sli.style.left = -this.screenWidth * (this.index - 2) + "px";
      this.$refs.sli.style.transition = "all 0.5s";
      this.index--;
      if (this.index == 1) {
        this.index = 3
        setTimeout(() => {
          this.$refs.sli.style.left = -this.screenWidth*this.index + "px";
          this.$refs.sli.style.transition = "all 0s";
          this.index = 4;
        }, 500);
      }
      this.autoLoop();
    }
  },
  created() {
    this.autoLoop();
  },
  mounted() {
    this.screenWidth = this.$refs.scr.clientWidth;
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.wrap {
  width: 1400px;
  height: 400px;
  margin: 100px auto;
  position: relative;
  .screen {
    width: 1280px;
    height: 400px;
    position: relative;
    left: 0;
    right: 0;
    margin: auto;
    overflow: hidden;
    .swiper-group {
      position: absolute;
      left: -1280px;
      top: 0;
      width: 500%;
      height: 400px;
      ul {
        width: 1280px;
        float: left;
        display: flex;
        justify-content: space-between;
        li {
          width: 635px;
          height: 400px;
          img {
            width: 100%;
            height: 100%;
          }
        }
      }
    }
  }
}
button {
  width: 50px;
  height: 30px;
  border: none;
  font-size:20px;
  line-height: 30px;
  text-align: center;
  border-radius:5px;
}
.left {
  position: absolute;
  top: 50%;
  left: -60px;
}
.right {
  position: absolute;
  top: 50%;
  right: -60px;
}
</style>
