<template>
  <div id="app">
      <div class="content">
        <div class="left-main">
          left
        </div>
        <div class="main">
          <p>配置3倍放大：<span style="color:#ff6600">:scale="3"</span></p>
          <div class="pic-box" id="pic-box">
            <pic-zoom url="static/imac2.jpg" positionbox="pic-box" :scale="3"></pic-zoom>
          </div>
          
          <p>配置放大时可滚动页面(滚轮)：<span style="color:#ff6600">:scroll="true"</span></p>
          <div class="pic-box" id="pic-box1">
            <pic-zoom :url="url" :scale="3" positionbox="pic-box1" :scroll="true"></pic-zoom>
          </div>
          <button @click="changeUrl">动态更改url</button>
          <p>分开配置大小图片地址：<span style="color:#ff6600">url="static/imac2_thumb.jpg" big-url="static/imac2.jpg"</span></p>
          <div class="pic-box" id="pic-box2">
            <pic-zoom url="static/imac2_thumb.jpg" big-url="static/imac2.jpg" positionbox="pic-box2" :scale="3" ></pic-zoom>
          </div>
        </div>
      </div>
      
  </div>
</template>

<script>
import PicZoom from './components/PicZoom'

export default {
  name: 'App',
  components: {
    PicZoom
  },
  data(){
    return{
      url:"static/imac2.jpg"
    }
  },
  methods:{
    changeUrl(){
      this.url="http://imgsrc.baidu.com/imgad/pic/item/3801213fb80e7beca9004ec5252eb9389b506b38.jpg"
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  padding-bottom: 100px;
}
.content{
  width: 100%;
  height: 100%;
}
.content .left-main{
  width: 300px;
  height: 100%;
  float: left;
}
.content .main{
  margin-left: 300px;
  margin-top: 30px;
  height: 100%;
}
.pic-box{
  width:500px;
  height:500px;
  border:1px solid #eee;
}
</style>
