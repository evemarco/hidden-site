<template lang="pug">
  //- .flex.flex-center
  div(style="overflow: hidden;")
    .video-background
      youtube(video-id="sPFII3ozSHI" host="http://www.youtube-nocookie.com" :mute="true" player-width="100%" player-height="100%" @ready="(player) => log(player)" @ended="(player) => player.target.playVideo()" :player-vars="{ autoplay: 1, loop: 1, controls: 0, showinfo:0, rel: 0, cc_load_policy: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, mute: 1, origin: '*' }").video-foreground
      .dark-background
    //- controls:0, showinfo:0, rel: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, loop: 1 }") cc_lang_pref: 'fr',
    #wrap.text-white
      section
        transition(appear enter-active-class="animated bounceInLeft delay-2s slow")
          h1.text-center(key="h1" style="font-family: 'Croissant One', cursive; padding-top: 10%;").no-wrap Hidden Baguette
        transition(appear enter-active-class="animated bounceInRight delay-2s slow")
          h2.text-center(key="h2") Corporation francophone
            br
            | Eve Online
            br
            | en wormhole space
      section.text-white
        h2.text-center Page 1
      section.text-white
        h2.text-center Page 2
      section.text-white
        h2.text-center Page 3
      section.text-white
        h2.text-center Page 4
</template>

<style>
/* * { box-sizing: border-box; } */
.dark-background {
  position: absolute;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.60);
  width: 100%;
  height: 100%;
}
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
/* @supports (object-fit: cover) {
  .video-foreground {
    top: 0; left: 0;
    width: 100%; height: 100%;
    object-fit: cover;
  }
} */
.page-container {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
#wrap {
  position: relative;
  overflow: hidden;
}
section {
  box-sizing: border-box;
  position: relative;
  height: 100vh;
  overflow: hidden;
  font-size: 10vmin;
  padding: 1em;
}
</style>

<script>
import webFont from 'webfontloader'
import * as FullPageScroll from 'responsive-fullpage-scroll/dist/fullpage-scroll'

export default {
  name: 'PageIndex',
  data () {
    return {
      url: window.location.href,
      FullPageScroll: FullPageScroll
    }
  },
  methods: {
    log (text) {
      // console.log(text)
    }
  },
  created () {
    webFont.load({
      google: {
        families: ['Croissant One'],
        text: 'Hidden Baguette'
      }
    })
  },
  mounted () {
    // console.log(FullPageScroll)
    let fps
    window.addEventListener('load', function () {
      fps = new this.FullPageScroll('wrap')
      document.addEventListener('keyup', function (event) {
        if (event.keyCode === 38) fps.previousSlide()
        else if (event.keyCode === 40) fps.nextSlide()
      })
    })
    this.$root.$on('goToSlide', (index) => {
      fps.goToSlide(index)
    })
  }
}
</script>
