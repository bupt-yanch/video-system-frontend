<template>
  <div class="my_video">
    <DropDown @process-change-event='processChange'></DropDown>
    <video-player  class="video-player vjs-custom-skin"
                   ref="videoPlayer"
                   :playsinline="true"
                   :options="videoPlayerOptions"
                   @play="onPlayerPlay($event)"
                   @pause="onPlayerPause($event)"
                   @ended="onPlayerEnded($event)"
                   @waiting="onPlayerWaiting($event)"
                   @playing="onPlayerPlaying($event)"
                   @loadeddata="onPlayerLoadeddata($event)"
                   @timeupdate="onPlayerTimeupdate($event)"
                   @canplay="onPlayerCanplay($event)"
                   @canplaythrough="onPlayerCanplaythrough($event)"
                   @statechanged="playerStateChanged($event)"
                   @ready="playerReadied"
    ></video-player>
    <div id='uploader'>
    <Uploader></Uploader>
    </div>
    <div id='submit_process'>
      <SubmitProcess></SubmitProcess>
    </div>
  </div>
</template>

<script>
// 导入组件
import {videoPlayer} from 'vue-video-player'
import Uploader from '@/components/videoProcess/Uploader.vue'
import DropDown from '@/components/videoProcess/DropDown.vue'
import SubmitProcess from '@/components/videoProcess/SubmitProcess.vue'
export default {
  name: 'VideoPlayer',
  components: {
    videoPlayer, Uploader, DropDown, SubmitProcess
  },
  data () {
    return {
      selectProcess: 'yeqiudi', // 具体视频
      fileType: 'mp4', // 资源的类型
      videoUrl: '', // 资源的路径地址
      posterUrl: '' // 封面地址
    }
  },
  mounted () {

  },
  methods: {
    processChange: function (selectProcess) {
      console.log('video_player caught process change', selectProcess)
      this.selectProcess = selectProcess
      this.$emit('process-change-event', selectProcess)
    },
    // 播放回调
    onPlayerPlay (player) {
      // console.log('player play!', player)
    },

    // 暂停回调
    onPlayerPause (player) {
      // console.log('player pause!', player)
    },

    // 视频播完回调
    onPlayerEnded ($event) {
      this.$refs.videoPlayer.player.src(this.fileUrl)
    },

    // DOM元素上的readyState更改导致播放停止
    onPlayerWaiting ($event) {
      // console.log(player)
    },

    // 已开始播放回调
    onPlayerPlaying ($event) {
      // console.log(player)
    },

    // 当播放器在当前播放位置下载数据时触发
    onPlayerLoadeddata ($event) {
      // console.log(player)
    },

    // 当前播放位置发生变化时触发。
    onPlayerTimeupdate ($event) {
      var videoSeconds = $event.cache_.currentTime
      this.$emit('video-seconds-change-event', videoSeconds)
    },

    // 媒体的readyState为HAVE_FUTURE_DATA或更高
    onPlayerCanplay (player) {
      // console.log('player Canplay!', player)
    },

    // 媒体的readyState为HAVE_ENOUGH_DATA或更高。这意味着可以在不缓冲的情况下播放整个媒体文件。
    onPlayerCanplaythrough (player) {
      // console.log('player Canplaythrough!', player)
    },

    // 播放状态改变回调
    playerStateChanged (playerCurrentState) {
      // console.log('player current update state', playerCurrentState)
    },

    // 将侦听器绑定到组件的就绪状态。与事件监听器的不同之处在于，如果ready事件已经发生，它将立即触发该函数。。
    playerReadied (player) {
      // console.log('example player 1 readied', player);
    }
  },
  computed: {
    videoPlayerOptions () {
      const videoPlayerOptions = {
        playbackRates: [0.75, 1.0, 1.25, 1.5, 2.0], // 播放速度
        autoplay: false, // 是否自动播放。
        muted: false, // 是否静音播放，默认情况下将会消除任何音频。
        loop: false, // 是否循环播放。
        preload: 'auto', // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: 'zh-CN',
        aspectRatio: '3:1', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 是否流体从而按比例缩放以适应其容器。
        flash: {hls: {withCreadentials: false}}, // 可以播放rtmp视频
        html5: {hls: {withCreadentials: false}}, // 可以播放m3u8视频
        sources: [{
          type: 'video/' + this.fileType,
          src: this.$axios.defaults.baseURL + '/file/video-preview?filePath=/2020110710/' + this.selectProcess.videoPath // 视频url地址
        }],
        poster: this.posterUrl, // 封面地址
        width: '100%',
        notSupportedMessage: '此视频暂无法播放...', // 当无法播放时允许覆盖Video.js，显示的默认信息。
        controlBar: {
          timeDivider: true,
          durationDisplay: true,
          remainingTimeDisplay: false,
          fullscreenToggle: true
        }
      }
      return videoPlayerOptions
    }
  }
}
</script>
<style scoped lang="less">
  .my_video{
    width: 100%;
    height: calc(100vh);
    background-color: white;
  }
  .video-js .vjs-big-play-button{
    /*对播放按钮的样式进行设置*/
    width: 100%;
    height: 100%;
    border-radius: 50%;
  }
  #uploader{
    margin-top: 20px;
  }
  .el-dropdown-link {
    cursor: pointer;
    color: #409EFF;
  }
  .el-icon-arrow-down {
    font-size: 12px;
  }
  .demonstration {
    display: block;
    color: #8492a6;
    font-size: 14px;
    margin-bottom: 20px;
  }
  #submit_process {
    margin-top: 40px;
  }
</style>
