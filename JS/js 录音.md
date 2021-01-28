#### 安装 js-audio-recorder

```
<template>
  <div class="home" style="margin:1vw;">
    <el-button type="success" @click="getPermission()" style="margin:1vw;">获取麦克风权限</el-button>
    <br/>
    <el-button type="info" @click="startRecorder()"  style="margin:1vw;">开始录音</el-button>
    <el-button type="info" @click="resumeRecorder()" style="margin:1vw;">继续录音</el-button>
    <el-button type="info" @click="pauseRecorder()" style="margin:1vw;">暂停录音</el-button>
    <el-button type="info" @click="stopRecorder()" style="margin:1vw;">结束录音</el-button>
    <br/>
    <el-button type="success" @click="playRecorder()" style="margin:1vw;">录音播放</el-button>
    <el-button type="success" @click="pausePlayRecorder()" style="margin:1vw;">暂停录音播放</el-button>
    <el-button type="success" @click="resumePlayRecorder()" style="margin:1vw;">恢复录音播放</el-button>
    <el-button type="success" @click="stopPlayRecorder()" style="margin:1vw;">停止录音播放</el-button>
    <br/>
    <el-button type="info" @click="getRecorder()" style="margin:1vw;">获取录音信息</el-button>
    <el-button type="info" @click="downPCM()" style="margin:1vw;">下载PCM</el-button>
    <el-button type="info" @click="downWAV()" style="margin:1vw;">下载WAV</el-button>
    <el-button type="info" @click="getMp3Data()" style="margin:1vw;">下载MP3</el-button>
    <br/>
    <el-button type="error" @click="destroyRecorder()" style="margin:1vw;">销毁录音</el-button>
  </div>
</template>

<script>
import Recorder from 'js-audio-recorder'
const lamejs = require('lamejs')
const recorder = new Recorder({
  sampleBits: 16,                 // 采样位数，支持 8 或 16，默认是16
  sampleRate: 48000,              // 采样率，支持 11025、16000、22050、24000、44100、48000，根据浏览器默认值，我的chrome是48000
  numChannels: 1,                 // 声道，支持 1 或 2， 默认是1
  // compiling: false,(0.x版本中生效,1.x增加中)  // 是否边录边转换，默认是false
})
 
// 绑定事件-打印的是当前录音数据
recorder.onprogress = function(params) {
  console.log('--------------START---------------')
  console.log('录音时长(秒)', params.duration);
  console.log('录音大小(字节)', params.fileSize);
  console.log('录音音量百分比(%)', params.vol);
  console.log('当前录音的总数据([DataView, DataView...])', params.data);

  console.log('--------------END---------------')
}
export default {
  name: 'home',
  methods: {
    /**
     *  录音的具体操作功能
     * */
    // 开始录音
    startRecorder () {
      recorder.start().then(() => {

      }, (error) => {
        // 出错了
        console.log(`${error.name} : ${error.message}`)
      })
    },
    // 继续录音
    resumeRecorder () {
      recorder.resume()
    },
    // 暂停录音
    pauseRecorder () {
      recorder.pause()
    },
    // 结束录音
    stopRecorder () {
      recorder.stop()
    },
    // 录音播放
    playRecorder () {
      recorder.play()
    },
    // 暂停录音播放
    pausePlayRecorder () {
      recorder.pausePlay()
    },
    // 恢复录音播放
    resumePlayRecorder () {
      recorder.resumePlay()
    },
    // 停止录音播放
    stopPlayRecorder () {
      recorder.stopPlay()
    },
    // 销毁录音
    destroyRecorder () {
      recorder.destroy().then(function() {
        recorder = null
      })
    },
    /**
     *  获取录音文件
     * */
    getRecorder(){
      let toltime = recorder.duration;//录音总时长
      let fileSize = recorder.fileSize;//录音总大小

      //录音结束，获取取录音数据
      let PCMBlob = recorder.getPCMBlob();//获取 PCM 数据
      let wav = recorder.getWAVBlob();//获取 WAV 数据

      let channel = recorder.getChannelData();//获取左声道和右声道音频数据
    },
    /**
     *  下载录音文件
     * */
    //下载pcm
    downPCM () {
      //这里传参进去的时文件名
      recorder.downloadPCM('新文件');
    },
    //下载wav
    downWAV () {
      //这里传参进去的时文件名
      recorder.downloadWAV('新文件');
    },
    /**
     *  获取麦克风权限
     * */
    getPermission () {
      Recorder.getPermission().then(() => {
        this.$Message.success('获取权限成功')
      }, (error) => {
        console.log(`${error.name} : ${error.message}`);
      });
    },
    /**
     * 文件格式转换 wav-map3
     * */
    getMp3Data () {
      const mp3Blob = this.convertToMp3(recorder.getWAV());
      recorder.download(mp3Blob, 'recorder', 'mp3');
    },
    convertToMp3(wavDataView) {
      // 获取wav头信息
      const wav = lamejs.WavHeader.readHeader(wavDataView) // 此处其实可以不用去读wav头信息，毕竟有对应的config配置
      const { channels, sampleRate } = wav
      const mp3enc = new lamejs.Mp3Encoder(channels, sampleRate, 128)
      // 获取左右通道数据
      const result = recorder.getChannelData()
      const buffer = []

      const leftData = result.left && new Int16Array(result.left.buffer, 0, result.left.byteLength / 2)
      const rightData = result.right && new Int16Array(result.right.buffer, 0, result.right.byteLength / 2)
      const remaining = leftData.length + (rightData ? rightData.length : 0)

      const maxSamples = 1152
      for (let i = 0; i < remaining; i += maxSamples) {
        const left = leftData.subarray(i, i + maxSamples)
        let right = null;
        let mp3buf = null;

        if (channels === 2) {
          right = rightData.subarray(i, i + maxSamples)
          mp3buf = mp3enc.encodeBuffer(left, right)
        } else {
          mp3buf = mp3enc.encodeBuffer(left)
        }

        if (mp3buf.length > 0) {
          buffer.push(mp3buf)
        }
      }
      const enc = mp3enc.flush()
      if (enc.length > 0) {
        buffer.push(enc);
      }
      return new Blob(buffer, { type: 'audio/mp3' });
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
```