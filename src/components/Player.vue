<template>
  <div class="flex player-component">
    <div class="flex player-header">
      <icon-back></icon-back>
      <div class="player-music-item">
        <h4 class="player-music-name text-ellipsis">{{ songMsg.name }}</h4>
        <p class="player-music-singer text-ellipsis">{{ songMsg.artists | transformArtistList }}</p>
      </div>
      <div class="player-share background"></div>
    </div>
    <div class="flex player-diskette">
      <div class="player-diskette-pole background" :class="{'playing': playStatus}"></div>
      <div class="player-diskette-bg"></div>
      <div class="flex player-diskette-rotate background" :class="{'rotating': playStatus}">
        <img class="player-diskette-album" :src="songMsg.album.picUrl" />
        <!--<img v-if="songMsg.album.picUrl !== ''" class="player-diskette-album" :src="songMsg.album.picUrl" />-->
        <!--<img v-else class="player-diskette-album" src="../../static/images/player/album.png" />-->
      </div>
    </div>
    <div class="player-footer">
      <div class="player-timeline"></div>
      <div class="flex player-controllers">
        <div @click="changePlayMode" class="player-mode background" :class="{'player-mode-listRandom': playMode === 'listRandom', 'player-mode-singleCycle': playMode === 'singleCycle'}"></div>
        <div @click="preControl" class="player-pre background"></div>
        <div @click="playControl" class="player-pause background" :class="{'player-play': !playStatus}"></div>
        <div @click="nextControl" class="player-next background"></div>
        <div class="player-playing-list background"></div>
      </div>
    </div>
  </div>
</template>
<style scoped>
  .player-component {
    height: 100%;
    flex-direction: column;
    background: linear-gradient(0deg, #6e564f, #5b1f1c);
  }
  .player-header {
    padding: 0.4375rem 0.875rem 0 1.125rem;
    height: 2.8125rem;
    justify-content: space-between;
    align-items: center;
  }
  .player-music-item {
    flex-grow: 1;
    padding-right: 1.125rem;
    padding-left: 1.125rem;
    overflow: hidden;
  }
  .player-music-name {
    color: #fff;
    font-size: 18px;
    line-height: 1.5rem;
  }
  [data-dpr="2"] .player-music-name {
    font-size: 36px;
  }
  [data-dpr="3"] .player-music-name {
    font-size: 54px;
  }
  .player-music-singer {
    color: #a5928e;
    font-size: 12px;
    line-height: 0.875rem;
  }
  [data-dpr="2"] .player-music-singer {
    font-size: 24px;
  }
  [data-dpr="3"] .player-music-singer {
    font-size: 36px;
  }
  .player-share {
    flex-shrink: 0;
    width: 1.25rem;
    height: 1.25rem;
    background-image: url("../../static/images/player/icon_share.png");
  }
  .player-diskette {
    position: relative;
    flex-grow: 1;
    justify-content: center;
    overflow: hidden;
  }
  .player-diskette-pole {
    position: absolute;
    top: -0.8125rem;
    left: 50%;
    z-index: 9;
    width: 5.25rem;
    height: 8.125rem;
    background-image: url("../../static/images/player/icon_diskette_pole.png");
    -webkit-transition: transform .5s;
    -moz-transition: transform .5s ;
    -ms-transition: transform .5s ;
    -o-transition: transform .5s ;
    transition: transform .5s ;
    transform-origin: 0.8125rem 0.8125rem;
    transform: rotate(-25deg);
  }
  .player-diskette-pole.playing {
    transform: rotate(5deg);
  }
  .player-diskette-bg,
  .player-diskette-rotate {
    position: absolute;
  }
  .player-diskette-bg {
    z-index: 1;
    top: 3.8125rem;
    width: 16.875rem;
    height: 16.875rem;
    background-color: rgba(255, 255, 255, .3);
    border-radius: 50%;
  }
  .player-diskette-rotate {
    z-index: 5;
    top: 4.125rem;
    width: 16.375rem;
    height: 16.375rem;
    justify-content: center;
    align-items: center;
    background-image: url("../../static/images/player/icon_diskette.png");
    border-radius: 50%;
    -webkit-animation: disketteRotate 20s linear infinite;
    -o-animation: disketteRotate 20s linear infinite;
    animation: disketteRotate 20s linear infinite;
    -webkit-animation-play-state: paused;
    -moz-animation-play-state: paused;
    -o-animation-play-state: paused;
    animation-play-state: paused;
  }
  .rotating {
    -webkit-animation-play-state: running;
    -moz-animation-play-state: running;
    -o-animation-play-state: running;
    animation-play-state: running;
  }
  @-webkit-keyframes disketteRotate {
    from {
      -webkit-transform: rotate3d(0, 0, 1, 0deg);
      -moz-transform: rotate3d(0, 0, 1, 0deg);
      -ms-transform: rotate3d(0, 0, 1, 0deg);
      -o-transform: rotate3d(0, 0, 1, 0deg);
      transform: rotate3d(0, 0, 1, 0deg);
    }
    to {
      -webkit-transform: rotate3d(0, 0, 1, 360deg);
      -moz-transform: rotate3d(0, 0, 1, 360deg);
      -ms-transform: rotate3d(0, 0, 1, 360deg);
      -o-transform: rotate3d(0, 0, 1, 360deg);
      transform: rotate3d(0, 0, 1, 360deg);
    }
  }
  @keyframes disketteRotate {
    from {
      -webkit-transform: rotate3d(0, 0, 1, 0deg);
      -moz-transform: rotate3d(0, 0, 1, 0deg);
      -ms-transform: rotate3d(0, 0, 1, 0deg);
      -o-transform: rotate3d(0, 0, 1, 0deg);
      transform: rotate3d(0, 0, 1, 0deg);
    }
    to {
      -webkit-transform: rotate3d(0, 0, 1, 360deg);
      -moz-transform: rotate3d(0, 0, 1, 360deg);
      -ms-transform: rotate3d(0, 0, 1, 360deg);
      -o-transform: rotate3d(0, 0, 1, 360deg);
      transform: rotate3d(0, 0, 1, 360deg);
    }
  }
  .player-diskette-album {
    width: 11.0625rem;
    height: 11.0625rem;
    border-radius: 50%;
  }
  .player-footer {
    height: 6.75rem;
  }
  .player-controllers {
    margin-top: 1.25rem;
    padding: 0 1.125rem;
    justify-content: space-between;
    align-items: center;
  }
  .player-mode,
  .player-playing-list {
    width: 1.375rem;
    height: 1.375rem;
  }
  .player-mode {
    background-image: url('../../static/images/player/icon_loop_mode.png');
  }
  .player-mode-listRandom {
    background-image: url('../../static/images/player/icon_random_mode.png');
  }
  .player-mode-singleCycle {
    background-image: url('../../static/images/player/icon_single_mode.png');
  }
  .player-playing-list {
    background-image: url('../../static/images/player/icon_playing_list.png');
  }
  .player-pre,
  .player-next {
    width: 1.5rem;
    height: 1.5rem;
  }
  .player-pre {
    background-image: url('../../static/images/player/icon_pre.png');
  }
  .player-next {
    background-image: url('../../static/images/player/icon_next.png');
  }
  .player-pause {
    width: 2.875rem;
    height: 2.875rem;
    background-image: url('../../static/images/player/icon_pause.png');
  }
  .player-play {
    background-image: url('../../static/images/player/icon_play.png');
  }
</style>
<script>
  import IconBack from './common/IconBack.vue'
  export default{
    data () {
      return {
      }
    },
    computed: {
      songMsg () {
        return this.$store.state.songMsg
      },
      playStatus () {
        return this.$store.state.playStatus
      },
      playMode () {
        return this.$store.state.playMode
      }
    },
    methods: {
      playControl () { //  播放控制
        if (this.playStatus) {
          this.$store.commit('pauseControl')
        } else {
          this.$store.commit('playControl')
        }
      },
      preControl () { //  上一首
        this.$store.dispatch('preControl')
      },
      nextControl () { //  下一首
        this.$store.dispatch('nextControl')
      },
      changePlayMode () {
        this.$store.commit('setPlayMode')
      }
    },
    created () {
    },
    components: {
      IconBack
    }
  }
</script>
