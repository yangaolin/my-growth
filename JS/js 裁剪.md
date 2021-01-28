安装 vue-cropper 然后main.js
```
import VueCropper from 'vue-cropper' 

Vue.use(VueCropper)
```
然后
```
<template>
  <div>
    <div class="cut">
      <vue-cropper ref="cropper" 
        :img="option.img" 
        :output-size="option.size" 
        :output-type="option.outputType" 
        :info="true" :full="option.full" 
        :fixed="fixed" 
        :fixed-number="fixedNumber"
        :can-move="option.canMove" 
        :can-move-box="option.canMoveBox" 
        :fixed-box="option.fixedBox" 
        :original="option.original"
        :auto-crop="option.autoCrop" 
        :auto-crop-width="option.autoCropWidth" 
        :auto-crop-height="option.autoCropHeight" 
        :center-box="option.centerBox"
				@real-time="realTime" 
        :high="option.high"
        @img-load="imgLoad" 
        mode="cover" 
        :max-img-size="option.max" 
        @crop-moving="cropMoving">
      </vue-cropper>
    </div>
     <div class="show-preview" :style="{'width': previews.w + 'px', 'height': previews.h + 'px',  'overflow': 'hidden', 'margin': '5px'}">
      <div :style="previews.div">
        <img :src="previews.url" :style="previews.img">
      </div>
    </div>
    <div class="test-button">
      <button @click="startCrop" v-if="!crap" class="btn">start</button>
      <button @click="stopCrop" v-else class="btn">stop</button>
      <button @click="clearCrop" class="btn">clear</button>
      <button @click="refreshCrop" class="btn">refresh</button>
      <button @click="finish" class="btn">preview(base64)</button>
      <a @click="down" class="btn">download(base64)</a>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      crap: false,
      previews: {},
      lists: [
        {
          img: 'https://qn-qn-kibey-static-cdn.app-echo.com/goodboy-weixin.PNG'
        },
        {
          img: 'https://avatars2.githubusercontent.com/u/15681693?s=460&v=4'
        }
      ],
      option: {
        img: 'http://localhost:9529/images/test.png',
        size: 1,
        full: false,
        outputType: 'png',
        canMove: false,
        fixedBox: false,
        original: true,
        canMoveBox: true,
        autoCrop: false,
        // 只有自动截图开启 宽度高度才生效
        autoCropWidth: 160,
        autoCropHeight: 150,
        centerBox: false,
        high: true,
        max: 99999,
        canScale: false
      },
      show: true,
      fixed: true,
      fixedNumber: [16, 9]
    }
  },
  methods: {
    startCrop() {
      // start
      this.crap = true
      this.$refs.cropper.startCrop()
    },
    stopCrop() {
      //  stop
      this.crap = false
      this.$refs.cropper.stopCrop()
    },
    clearCrop() {
      // clear
      this.$refs.cropper.clearCrop()
    },
    refreshCrop() {
      // clear
      this.$refs.cropper.refresh()
    },
    finish() {
      this.$refs.cropper.getCropData((data) => {
        console.log(data)
      })
    },
    // 实时预览函数
    realTime(data) {
      this.previews = data
      console.log(data)
    },
    down() {
      // event.preventDefault()
      var aLink = document.createElement('a')
      aLink.download = 'demo'
      // 输出
      this.$refs.cropper.getCropData((data) => {
        this.downImg = data
        aLink.href = data
        aLink.click()
      })
    },
    imgLoad(msg) {
      console.log(msg)
    },
    cropMoving(data) {
      console.log(data, '截图框当前坐标')
    }
  }
}
</script>

<style lang="scss">
.cut {
  width: 500px;
  height: 500px;
  margin: 30px auto;
}
.c-item {
  max-width: 800px;
  margin: 10px auto;
  margin-top: 20px;
}
.content {
  margin: auto;
  max-width: 1200px;
  margin-bottom: 100px;
}
.test-button {
  display: flex;
  flex-wrap: wrap;
  align-content: center;
  justify-content: center;
}
.btn {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  border: 1px solid #c0ccda;
  color: #1f2d3d;
  text-align: center;
  box-sizing: border-box;
  outline: none;
  margin:20px 10px 0px 0px;
  padding: 9px 15px;
  font-size: 14px;
  border-radius: 4px;
  color: #fff;
  background-color: #50bfff;
  border-color: #50bfff;
  transition: all .2s ease;
  text-decoration: none;
  user-select: none;
}
.des {
  line-height: 30px;
}
code.language-html {
  padding: 10px 20px;
  margin: 10px 0px;
  display: block;
  background-color: #333;
  color: #fff;
  overflow-x: auto;
  font-family: Consolas, Monaco, Droid, Sans, Mono, Source, Code, Pro, Menlo, Lucida, Sans, Type, Writer, Ubuntu, Mono;
  border-radius: 5px;
  white-space: pre;
}

.show-info {
  margin-bottom: 50px;
}

.show-info h2 {
  line-height: 50px;
}

/*.title, .title:hover, .title-focus, .title:visited {
  color: black;
}*/

.title {
  display: block;
  text-decoration: none;
  text-align: center;
  line-height: 1.5;
  margin: 20px 0px;
  background-image: -webkit-linear-gradient(left,#3498db,#f47920 10%,#d71345 20%,#f7acbc 30%,#ffd400 40%,#3498db 50%,#f47920 60%,#d71345 70%,#f7acbc 80%,#ffd400 90%,#3498db);
  color: transparent;
  -webkit-background-clip: text;
  background-size: 200% 100%;
  animation: slide 5s infinite linear;
  font-size: 40px;
}

.test {
  height: 500px;
}

.model {
  position: fixed;
  z-index: 10;
  width: 100vw;
  height: 100vh;
  overflow: auto;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.8);
}

.model-show {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}

.model img {
  display: block;
  margin: auto;
  max-width: 80%;
  user-select: none;
  background-position: 0px 0px, 10px 10px;
  background-size: 20px 20px;
  background-image: linear-gradient(45deg, #eee 25%, transparent 25%, transparent 75%, #eee 75%, #eee 100%),linear-gradient(45deg, #eee 25%, white 25%, white 75%, #eee 75%, #eee 100%);
}

.c-item {
  display: block;
  user-select: none;
}

@keyframes slide {
  0%  {
    background-position: 0 0;
  }
  100% {
    background-position: -100% 0;
  }
}
    
</style>
```