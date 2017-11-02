<template>
  <img src="./img/cover.png" height="387" width="563" v-show="cover" @click="play()" class="cover">
  <!-- 视频video -->
  <div class="videobox">
    <video
      id="video1" 
      :src="src_mp4"
      preload="auto"
      webkit-playsinline
      playsinline 

      x-webkit-airplay="allow" 
      x5-video-player-type="h5"  
      x5-video-player-fullscreen="true"
      x5-video-orientation="landscape"
      style="object-fit:fill">
    </video>
  </div>
</template>
<script>
  import inline from './js/inline.js';
  var mp4 = require('./main.mp4');
  export default{
    data() {
      return{
        cover: true,
        src_mp4: mp4,
      }
    },
    ready() {
      var video = document.querySelector('video');

      this.videoMethod(video);
    },
    methods:{
      videoMethod(video) {
        var that = this;
        video.addEventListener('timeupdate',function (){
          if ( !video.isPlayed && this.currentTime>0.1 ){
            _hmt.push(['_trackPageview', '/activity/celebration/play']);
            video.isPlayed = !0;
            that.cover = false;
          }
        })
        //进入全屏
        video.addEventListener("x5videoenterfullscreen", function(){
          that.cover = false;
          window.onresize = function(){
            video.style.width = window.innerWidth + "px";
            video.style.height = window.innerHeight + "px";
          }
        })
        //退出全屏
        video.addEventListener("x5videoexitfullscreen", function(){
          window.onresize = function(){
            video.style.width = 5.54 + "rem";
            video.style.height = 3.34 + "rem";
          }
          that.cover = true;
        })
        //播放结束
        video.addEventListener('ended', function (e) {
          that.cover = true;
        })
      },
      play() {
        var video = document.querySelector('video');
        video.play();
        this.cover = false;
        // 未播放点击记录
        if (this.cover) {
          _hmt.push(['_trackPageview', '/activity/celebration/playBtn']);
        }
        setTimeout(function () { video.play(); }, 1000);
        document.addEventListener("WeixinJSBridgeReady", function (){ 
            video.play();
            video.pause();
        }, false);
      }
    }
  }
</script>
<style lang="scss">
html,
body{
  margin: 0;
  padding: 0;
  background: #f9d2cd;
}
.cover{
  width: 5.63rem;
  height: 3.87rem;
  position: absolute;
  top: 3.32rem;
  left: 1.06rem;
  z-index: 4;
}
// video
.IIV::-webkit-media-controls-play-button,
.IIV::-webkit-media-controls-start-playback-button {
  opacity: 0;
  pointer-events: none;
  width: 5px;
}
.videobox {
  width: 5.1rem;
  height: 3.8rem;
  position: absolute;
  top: 3.32rem;
  left: 1.06rem;
  z-index: 2;
  background-color: black;
}
video{
  width: 5.1rem;
  height: 3.8rem;
  overflow: hidden;
}
</style>