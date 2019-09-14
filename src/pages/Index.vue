<template lang="pug">
  .flex.flex-center
    youtube(video-id="sPFII3ozSHI" style="position: fixed; z-index: -99; width: 100%; height: 100%; top: 0;" :mute="true" player-width="100%" player-height="100%" @ready="(player) => log(player)" @ended="(player) => player.target.playVideo()" :player-vars="{ autoplay: 1, loop: 1, controls: 0, showinfo:0, rel: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1 }")
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
