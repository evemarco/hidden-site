<template lang="pug">
  .flex.flex-center
    .video-background
      youtube(video-id="sPFII3ozSHI" :mute="true" player-width="100%" player-height="100%" @ready="(player) => log(player)" @ended="(player) => player.target.playVideo()" :player-vars="{ autoplay: 1, loop: 1, controls: 0, showinfo:0, rel: 0, cc_load_policy: 0, cc_lang_pref: 'fr', iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1 }").video-foreground
    //- controls:0, showinfo:0, rel: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, loop: 1 }")
    div(style="width: calc(100vw); height:calc(100vh - 50px); background-color: rgba(0 ,0 ,0 ,0.65)").text-white
      h1.text-center(style="font-family: 'Croissant One', cursive; padding-top: 10%;") Hidden Baguette
      h2.text-center Corporation francophone
        br
        | Eve Online
        br
        | en wormhole space
</template>

<style>
/* * { box-sizing: border-box; } */
.video-background {
  background: #000;
  position: fixed;
  top: 0; right: 0; bottom: 0; left: 0;
  z-index: -99;
}
.video-foreground,
.video-background iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
}
@media (min-aspect-ratio: 16/9) {
  .video-foreground { height: 300%; top: -100%; }
}
@media (max-aspect-ratio: 16/9) {
  .video-foreground { width: 300%; left: -100%; }
}
@supports (object-fit: cover) {
  .video-background {
    top: 0; left: 0;
    width: 100%; height: 100%;
    object-fit: cover;
  }
}
</style>

<script>
import webFont from 'webfontloader'

export default {
  name: 'PageIndex',
  methods: {
    log (text) {
      console.log(text)
    }
  },
  created () {
    webFont.load({
      google: {
        families: ['Croissant One'],
        text: 'Hidden Baguette'
      }
    })
  }
}
</script>
