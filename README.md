# vue-imagezoom 基于 vue-piczoom

基于 [ vue-piczoom 项目修改](https://github.com/826327700/vue-piczoom) 

#所有权限归原作者所有

> A picture magnifier component for Vue.js 2.x
基于vue2.x的电商图片放大镜插件

### Build Setup 使用步骤

``` bash
# 安装 install
yarn add vue-imagezoom --save
```
``` bash
# 使用 use
--script
import ImageZoom from 'vue-imagezoom'
export default {
  name: 'App',
  components: {
    ImageZoom
  }
}

--html
<pic-zoom url="static/imac2.jpg" :scale="3"></pic-zoom>
```
### Config 配置
props | describe | default
----|------|----
url | 图片地址  | string required
big-url | 大图地址 | string null
scale | 图片放大倍数  | number 2.5 
scroll | 放大时页面是否可滚动  | boolean fasle 
positionbox | 基于此父元素定位  | string #ID required

### Suggest 注意事项
组件默认是100%的高宽，所以建议将组件包含在一个有固定高宽的容器内。如：
```
<div class="pic-box"> <!--pic-box:width:500px;height:500px-->
     <pic-zoom url="static/imac2.jpg" :scale="3"></pic-zoom>
</div>
```
### GIF 动画截图
![zoom2.gif](http://upload-images.jianshu.io/upload_images/6651371-e26a702c2ef8651a.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Demo 示例
[在线示例](https://826327700.github.io/vue-imagezoom/dist/ "图片放大镜")

### Modify 修改

Jeffery G.