<template lang="pug">
  //- .flex.flex-center
  div(style="overflow: hidden;")
    .video-background
      youtube(v-if="id_youtube" :video-id="id_youtube" host="http://www.youtube-nocookie.com" :mute="true" player-width="100%" player-height="100%" @ready="(player) => ytReady(player)" @ended="(player) => ytReplay(player)" :player-vars="{ autoplay: 1, loop: 1, start: debut, end: fin, controls: 0, showinfo:0, rel: 0, cc_load_policy: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, mute: 1, origin: '*' }").video-foreground
      .dark-background(:style="transparence > 0 ? `background-color: rgba(0, 0, 0, ${transparence});` : ''")
    //- controls:0, showinfo:0, rel: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, loop: 1 }") cc_lang_pref: 'fr',
    #wrap.text-white
      section.flex.flex-center
        .self-center(style="margin-top: -50px;")
          transition(appear enter-active-class="animated bounceInLeft")
            .text-center(key="h1" :style="font_logo ? `font-family: ${font_logo}, cursive;` : ''").row.full-width.size-titre
              .fit.row.justify-center.items-center.content-center
                img(key="img" :src="image_logo" style="max-width: 16vmin; max-height: 16vmin;").col-shrink.on-left.animated.myanim.pulse
                span(style="word-wrap: break-word;").col-shrink {{ nom_corpo }}
          transition(appear enter-active-class="animated bounceInRight")
            .text-center(key="h2" :style="font_sous_titre ? `font-family: ${font_sous_titre}` : ''" v-html="sous_titre").size-sous-titre
      section.text-white
        .self-center(style="margin-top: -50px; background-color: rgba(0,0,0,0.5); padding: 1vh 1vw;")
          .text-center.size-titre(:style="`font-family: ${font_bouton}`") {{ bouton_presentation }}
          div(:style="`font-family: ${font_texte_normal}`" v-html="presentation").size-texte
      section.text-white
        .self-center(style="margin-top: -50px; background-color: rgba(0,0,0,0.5); padding: 1vh 1vw;")
          .text-center.size-titre(:style="`font-family: ${font_bouton};`") {{ bouton_activites }}
          //- div(style="height: calc(72vh - 50px);")
          vue-flux(:options="activitesOptions" :images="activitesImages" :transitions="activitesTransitions" :captions="activitesCaptions" ref="activites")
            template(v-slot:preloader)
              flux-preloader
            template(v-slot:caption)
              flux-caption(v-slot="captionProps")
                .flux-caption(v-html="captionProps.text")
            template(v-slot:controls)
              flux-controls
            template(v-slot:pagination)
              flux-pagination
            //- template(v-slot:index)
            //-   flux-index
      section.text-white
        .self-center(style="margin-top: -50px;")
          .text-center.size-titre(:style="`font-family: ${font_bouton}`") {{ bouton_membres }}
      section.text-white
        .self-center(style="margin-top: -50px; background-color: rgba(0,0,0,0.5); padding: 1vh 1vw;")
          .text-center.size-titre(:style="`font-family: ${font_bouton}`")
            img(src="https://cdn.pixabay.com/photo/2012/04/25/08/56/america-41776_960_720.png").img-titre
            span.q-mx-lg {{ recrutement_titre }}
            img(src="https://cdn.pixabay.com/photo/2012/04/25/08/56/america-41776_960_720.png").img-titre
          q-img(:src="recrutement_image_de_fond" style="height: calc(72vh - 50px);" position="center center")
            div(:style="`font-family: ${font_texte_normal};`" v-html="recrutement_descriptif").size-texte.absolute-full.text-subtitle2.flex.flex-center.column
      section.text-white
        .self-center(style="margin-top: -50px;")
          .text-center.size-titre(:style="`font-family: ${font_bouton}`") {{ bouton_outils }}
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
.size-titre {
  font-size: 8vmin;
}
.size-sous-titre {
  font-size: 5vmin;
}
.size-texte {
  font-size: 2vmin;
}
.img-titre {
  height: 8vmin;
}
.vue-flux {
  height: calc(72vh - 50px);
}
.flux-caption-titre {
  font-size: 4vmin;
}
.flux-caption-descriptif {
  font-size: 2vmin;
}
</style>

<script>
import webFont from 'webfontloader'
import * as FullPageScroll from 'responsive-fullpage-scroll/dist/fullpage-scroll'
import { DeliveryClient } from '@kentico/kontent-delivery'
import { VueFlux, FluxCaption, FluxControls, FluxPagination, FluxPreloader } from 'vue-flux'

const deliveryClient = new DeliveryClient({
  projectId: '66e72765-32bf-0056-dae3-bb6d6dc6131f',
  previewApiKey: 'ew0KICAiYWxnIjogIkhTMjU2IiwNCiAgInR5cCI6ICJKV1QiDQp9.ew0KICAianRpIjogIjk0ZWZmMGJkNzJmOTQ0OTliYmVhNjYwN2E2MWYxY2U0IiwNCiAgImlhdCI6ICIxNTY5MzIxOTMyIiwNCiAgImV4cCI6ICIxOTE0OTIxOTMyIiwNCiAgInByb2plY3RfaWQiOiAiNjZlNzI3NjUzMmJmMDA1NmRhZTNiYjZkNmRjNjEzMWYiLA0KICAidmVyIjogIjEuMC4wIiwNCiAgImF1ZCI6ICJwcmV2aWV3LmRlbGl2ZXIua2VudGljb2Nsb3VkLmNvbSINCn0.OdNPgrvPBeRMiIT1vCy0LphwVcFhCbsB2otm9GT302M',
  globalQueryConfig: {
    usePreviewMode: true // Queries the Delivery Preview API.
  }
})

export default {
  name: 'PageIndex',
  components: {
    VueFlux,
    FluxCaption,
    FluxControls,
    // FluxIndex,
    FluxPagination,
    FluxPreloader
  },
  data () {
    return {
      url: window.location.href,
      FullPageScroll: FullPageScroll,
      nom_corpo: '',
      sous_titre: '',
      image_logo: '',
      bouton_presentation: '',
      presentation: '',
      bouton_activites: '',
      bouton_membres: '',
      bouton_recrutement: '',
      bouton_outils: '',
      font_bouton: '',
      font_logo: '',
      font_sous_titre: '',
      font_texte_normal: '',
      id_youtube: '',
      transparence: 0,
      debut: 0,
      fin: null,
      player: {},
      activitesOptions: { autoplay: true, delay: 10000 },
      activitesImages: [],
      activitesTransitions: [ 'concentric', 'warp', 'round1', 'round2', 'fade', 'wave' ],
      activitesCaptions: [],
      recrutement_titre: '',
      recrutement_descriptif: '',
      recrutement_image_de_fond: ''
    }
  },
  methods: {
    log (text) {
      // console.log(text)
    },
    ytReplay (player) {
      // console.log(player)
      player.target.seekTo(this.debut)
      player.target.playVideo()
    },
    ytReady (player) {
      // console.log(player)
      this.player = player.target
      // this.player.loadVideoById({ 'videoId': this.id_youtube, 'startSeconds': this.debut, 'endSeconds': this.fin })
      // console.log('player', this.player)
      // this.$root.$on('youtube', (id) => {
      //   player.loadVideoById({ 'videoId': id, 'startSeconds': this.debut, 'endSeconds': this.fin })
      //   console.log(player)
      // })
    }
  },
  created () {
    // webFont.load({
    //   google: {
    //     families: ['Croissant One']
    //     // text: 'Hidden Baguette'
    //   }
    // })
    deliveryClient.items().type('site').toPromise().then(response => {
      let items = response.items[0]
      this.$root.$emit('site', items)
      this.font_bouton = items.font_bouton.value
      this.font_logo = items.font_logo.value
      this.font_sous_titre = items.font_sous_titre.value
      this.font_texte_normal = items.font_texte_normal.value
      let listeFonts = []
      if (this.font_bouton) listeFonts.push(this.font_bouton)
      if (this.font_logo) listeFonts.push(this.font_logo)
      if (this.font_sous_titre) listeFonts.push(this.font_sous_titre)
      if (this.font_texte_normal) listeFonts.push(this.font_texte_normal)
      if (listeFonts.length > 0) {
        webFont.load({
          google: {
            families: listeFonts
          }
        })
      }
      this.nom_corpo = items.nom_corpo.value
      this.sous_titre = items.sous_titre.value
      this.image_logo = items.image_logo.value[0].url
      this.debut = items.debut.value
      this.fin = items.fin.value
      this.id_youtube = items.id_youtube.value
      // this.player.loadVideoById({ 'videoId': this.id_youtube, 'startSeconds': this.debut, 'endSeconds': this.fin })
      // console.log('player', this.player)
      // this.$root.$emit('youtube', this.id_youtube)
      this.transparence = items.transparence.value
      this.bouton_presentation = items.bouton_presentation.value
      this.presentation = items.presentation.value
      this.bouton_activites = items.bouton_activites.value
      this.bouton_membres = items.bouton_membres.value
      this.bouton_recrutement = items.bouton_recrutement.value
      this.bouton_outils = items.bouton_outils.value
      this.recrutement_titre = items.diapo__titre.value
      this.recrutement_descriptif = items.diapo__descriptif.value
      this.recrutement_image_de_fond = items.diapo__image_de_fond.value[0].url
      for (const activite of items.activites.value) {
        this.activitesImages.push(activite.diapo__image_de_fond.value[0].url)
        this.activitesCaptions.push(`<div class="flux-caption-titre">${activite.diapo__titre.value}</div><div class="flux-caption-descriptif">${activite.diapo__descriptif.value}</div>`)
      }
      // console.log(items)
    })
  },
  computed: {
    idYoutube () {
      if (this.id_youtube && this.player) return this.id_youtube
      else return ''
    }
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
