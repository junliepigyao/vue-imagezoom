<template>
    <div class="magnifier-box" :class="vertical?'vertical':''" :ref="id" @mousemove="mousemove" @mouseover="mouseover" @mouseleave="mouseleave">
        <img v-show="showImg" :src="imgUrl" alt="">
    </div>
</template>

<script>
export default {
  props: {
    scale: {
      type: Number,
      default: 2.5
    },
    positionbox: {
      type: String,
      required: true
    },
    url: {
      type: String,
      required: true
    },
    bigUrl: {
      type: String,
      default: null
    },
    scroll: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      id: null,
      cover: null,
      imgbox: null,
      imgwrap: null,
      orginUrl: null,
      bigImgUrl: null,
      bigOrginUrl: null,
      imgUrl: null,
      img: null,
      canvas: null,
      ctx: null,
      rectTimesX: 0,
      rectTimesY: 0,
      imgTimesX: 0,
      imgTimesY: 0,
      init: false,
      step: 0,
      bigStep: 0,
      vertical: false,
      showImg: true
    };
  },
  created() {
    var $chars =
      "ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678"; /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
    var maxPos = $chars.length;
    var str = "";
    for (let i = 0; i < 10; i++) {
      str += $chars.charAt(Math.floor(Math.random() * maxPos));
    }
    this.id = str;
    this.imgUrl = this.url;
    this.orginUrl = this.url;
    this.bigImgUrl = this.bigUrl;
    this.bigOrginUrl = this.bigUrl;
  },
  watch: {
    url: function(val) {
      this.imgUrl = val;
      this.orginUrl = val;
      this.initTime();
    },
    bigUrl: function() {
      this.bigImgUrl = bigUrl;
      this.bigOrginUrl = bigUrl;
      this.initTime();
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initTime();
    });
  },
  methods: {
    initTime() {
      this.init = false;
      let box = this.$refs[this.id];
      this.imgbox = box;
      this.imgwrap = box.querySelector("img");
      this.cover = {
          X:(this.imgbox.offsetWidth/this.scale),
          Y:(this.imgbox.offsetHeight/this.scale)
      }
      let imgsrc;
      if (this.bigImgUrl) {
        imgsrc = this.bigImgUrl;
      } else {
        imgsrc = this.imgUrl;
      }

      this.img = new Image();
      this.img.src = imgsrc;
      this.img.onload = () => {
        (this.rectTimesX =
          this.imgbox.offsetWidth / this.scale / this.imgwrap.offsetWidth),
          (this.rectTimesY =
            this.imgbox.offsetHeight / this.scale / this.imgwrap.offsetHeight);
        (this.imgTimesX = this.img.width / this.imgwrap.offsetWidth),
          (this.imgTimesY = this.img.height / this.imgwrap.offsetHeight);
        this.vertical = this.img.width < this.img.height;
        this.init = true;
      };
      if (this.canvas) {
        this.canvas.parentNode.removeChild(this.canvas);
        this.canvas = null;
      }
      this.canvas = document.createElement("canvas");
      this.canvas.className = "mouse-cover-canvas";
      this.canvas.style.position = "absolute";
      let positionGet = document.querySelector("#" + this.positionbox);
      this.canvas.style.left =
        positionGet.offsetLeft + this.imgbox.offsetWidth + 10 + "px";
      this.canvas.style.top = positionGet.offsetTop + "px";
      this.canvas.style.border = "1px solid #eee";
      this.canvas.style.zIndex = "99999";
      this.canvas.height = this.imgbox.offsetHeight;
      this.canvas.width = this.imgbox.offsetWidth;
      this.canvas.style.display = "none";
      document.body.append(this.canvas);
      this.ctx = this.canvas.getContext("2d");
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    },
    initBox() {
      this.showImg = false;
      this.canvas.style.display = "none";
      let box = this.$refs[this.id];
      let imgsrc;
      if (this.bigImgUrl) {
        imgsrc = this.bigImgUrl;
      } else {
        imgsrc = this.imgUrl;
      }
      this.img = new Image();
      this.img.src = imgsrc;
      this.img.onload = () => {
        this.vertical = this.img.width < this.img.height;
        this.showImg = true;
        let thumb = box.querySelector("img");
        setTimeout(() => {
          (this.rectTimesX =
            this.imgbox.offsetWidth /
            this.scale /
            box.querySelector("img").offsetWidth),
            (this.rectTimesY =
              this.imgbox.offsetHeight /
              this.scale /
              box.querySelector("img").offsetHeight);
        }, 20);
      };
    },
    mousemove(e) {
      if (!this.init) {
        return false;
      }
      let _this = this;
      //获取实际的offset
      function offset(curEle) {
        var totalLeft = null,
          totalTop = null,
          par = curEle.offsetParent;
        //首先加自己本身的左偏移和上偏移
        totalLeft += curEle.offsetLeft;
        totalTop += curEle.offsetTop;
        //只要没有找到body，我们就把父级参照物的边框和偏移也进行累加
        while (par) {
          if (navigator.userAgent.indexOf("MSIE 8.0") === -1) {
            //累加父级参照物的边框
            totalLeft += par.clientLeft;
            totalTop += par.clientTop;
          }

          //累加父级参照物本身的偏移
          totalLeft += par.offsetLeft;
          totalTop += par.offsetTop;
          par = par.offsetParent;
        }

        return {
          left: totalLeft,
          top: totalTop
        };
      }

      function getXY(eve) {
        return {
          x: eve.clientX - (_this.cover.X/2),
          y: eve.clientY - (_this.cover.Y/2)
        };
      }
      let oEvent = e || event;
      let pos = getXY(oEvent);
      let imgwrap = offset(this.imgwrap);
      this.ctx.clearRect(
        0,
        0,
        this.imgwrap.offsetWidth,
        this.imgwrap.offsetHeight
      );
      let startX = pos.x - (imgwrap.left - document.documentElement.scrollLeft),
        startY = pos.y - (imgwrap.top - document.documentElement.scrollTop);
      this.ctx.drawImage(
        this.img,
        startX * this.imgTimesX,
        startY * this.imgTimesY,
        this.img.width * this.rectTimesX,
        this.img.height * this.rectTimesY,
        0,
        0,
        this.imgbox.offsetWidth,
        this.imgbox.offsetHeight
      );
    },
    mouseover(e) {
      if (!this.init) {
        return false;
      }
      e = e || event;
      if (!this.scroll) {
        e.currentTarget.addEventListener(
          "mousewheel",
          function(ev) {
            ev.preventDefault();
          },
          false
        );

        e.currentTarget.addEventListener(
          "DOMMouseScroll",
          function(ev) {
            ev.preventDefault();
          },
          false
        );
      }

      this.canvas.style.display = "block";
    },
    mouseleave() {
      if (!this.init) {
        return false;
      }
      this.canvas.style.display = "none";
    }
  }
};
</script>

<style lang="scss" scoped>
.magnifier-box {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  position: relative;
  img {
    width: 100%;
  }
  .mouse-cover {
    position: fixed;
    background-color: rgba(0, 0, 0, 0.5);
    cursor: move;
  }
  .mouse-cover-canvas {
    position: fixed;
    left: 100%;
    top: 0;
    width: 100%;
    height: 100%;
  }
  &.vertical {
    img {
      height: 100%;
      width: auto;
    }
  }
}
</style>

