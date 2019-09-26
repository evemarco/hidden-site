<template lang="pug">
  //- .flex.flex-center
  div(style="overflow: hidden;")
    .video-background
      youtube(:video-id="id_youtube" host="http://www.youtube-nocookie.com" :mute="true" player-width="100%" player-height="100%" @ready="(player) => ytReady(player)" @ended="(player) => player.target.playVideo()" :player-vars="{ autoplay: 1, loop: 1, controls: 0, showinfo:0, rel: 0, cc_load_policy: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, mute: 1, origin: '*' }").video-foreground
      .dark-background(:style="transparence > 0 ? `background-color: rgba(0, 0, 0, ${transparence});` : ''")
    //- controls:0, showinfo:0, rel: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, loop: 1 }") cc_lang_pref: 'fr',
    #wrap.text-white
      section.flex.flex-center
        .self-center(style="margin-top: -50px;")
          transition(appear enter-active-class="animated bounceInLeft")
            .text-center(key="h1" style="font-family: 'Croissant One', cursive; font-size: 8vmin;").row.full-width
              .fit.row.justify-center.items-center.content-center
                img(key="img" :src="image_logo" style="max-width: 16vmin; max-height: 16vmin;").col-shrink.on-left.animated.myanim.pulse
                span(style="word-wrap: break-word;").col-shrink {{ nom_corpo }}
          transition(appear enter-active-class="animated bounceInRight")
            .text-center(key="h2" style="font-size: 5vmin;" v-html="sous_titre")
      section.text-white
        h2.text-center {{ bouton_presentation }}
      section.text-white
        h2.text-center {{ bouton_activites }}
      section.text-white
        h2.text-center {{ bouton_membres }}
      section.text-white
        h2.text-center {{ bouton_recrutement }}
      section.text-white
        h2.text-center {{ bouton_outils }}
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
.myanim {
  animation-duration: 4s;
  animation-delay: 2s;
  animation-iteration-count: infinite;
}
</style>

<script>
import webFont from 'webfontloader'
import * as FullPageScroll from 'responsive-fullpage-scroll/dist/fullpage-scroll'
import { DeliveryClient } from '@kentico/kontent-delivery'

const deliveryClient = new DeliveryClient({
  projectId: '66e72765-32bf-0056-dae3-bb6d6dc6131f',
  previewApiKey: 'ew0KICAiYWxnIjogIkhTMjU2IiwNCiAgInR5cCI6ICJKV1QiDQp9.ew0KICAianRpIjogIjk0ZWZmMGJkNzJmOTQ0OTliYmVhNjYwN2E2MWYxY2U0IiwNCiAgImlhdCI6ICIxNTY5MzIxOTMyIiwNCiAgImV4cCI6ICIxOTE0OTIxOTMyIiwNCiAgInByb2plY3RfaWQiOiAiNjZlNzI3NjUzMmJmMDA1NmRhZTNiYjZkNmRjNjEzMWYiLA0KICAidmVyIjogIjEuMC4wIiwNCiAgImF1ZCI6ICJwcmV2aWV3LmRlbGl2ZXIua2VudGljb2Nsb3VkLmNvbSINCn0.OdNPgrvPBeRMiIT1vCy0LphwVcFhCbsB2otm9GT302M',
  globalQueryConfig: {
    usePreviewMode: true // Queries the Delivery Preview API.
  }
})

export default {
  name: 'PageIndex',
  data () {
    return {
      url: window.location.href,
      FullPageScroll: FullPageScroll,
      nom_corpo: '',
      sous_titre: '',
      image_logo: '',
      bouton_presentation: '',
      bouton_activites: '',
      bouton_membres: '',
      bouton_recrutement: '',
      bouton_outils: '',
      id_youtube: '',
      transparence: 0
    }
  },
  methods: {
    log (text) {
      // console.log(text)
    },
    ytReady (player) {
      // this.$root.$on('youtube', (id) => {
      //   player.loadVideoById({ 'videoId': id, 'startSeconds': 5, 'endSeconds': 60 })
      // })
    }
  },
  created () {
    webFont.load({
      google: {
        families: ['Croissant One'],
        text: 'Hidden Baguette'
      }
    })
    deliveryClient.items().type('site').toPromise().then(response => {
      let items = response.items[0]
      this.$root.$emit('site', items)
      this.nom_corpo = items.nom_corpo.value
      this.sous_titre = items.sous_titre.value
      this.image_logo = items.image_logo.value[0].url
      this.id_youtube = items.id_youtube.value
      this.transparence = items.transparence.value
      this.bouton_presentation = items.bouton_presentation.value
      this.bouton_activites = items.bouton_activites.value
      this.bouton_membres = items.bouton_membres.value
      this.bouton_recrutement = items.bouton_recrutement.value
      this.bouton_outils = items.bouton_outils.value
      // console.log(items)
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
