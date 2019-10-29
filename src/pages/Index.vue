<template lang="pug">
  .hidden-overflow
    //- Fond vidéo youtube
    .video-background
      youtube(v-if="id_youtube" :video-id="id_youtube" host="https://www.youtube-nocookie.com" :mute="true" player-width="100%" player-height="100%" @ready="(player) => ytReady(player)" @ended="(player) => ytReplay(player)" :player-vars="{ autoplay: 1, loop: 1, start: debut, end: fin, controls: 0, showinfo:0, rel: 0, cc_load_policy: 0, iv_load_policy: 3, disablekb: 1, fs: 0, modestbranding: 1, autohide: 1, mute: 1, origin: '*' }").video-foreground
      .dark-background(:style="transparence > 0 ? `background-color: rgba(0, 0, 0, ${transparence});` : ''")
    #wrap.text-white
      //- Section accueil
      section.flex.flex-center
        .self-center(style="margin-top: -50px;")
          transition(appear enter-active-class="animated bounceInLeft")
            .text-center(key="h1" :style="font_logo ? `font-family: ${font_logo}, cursive;` : ''").row.full-width.size-titre
              .fit.row.justify-center.items-center.content-center
                img(key="img" :src="image_logo" style="max-width: 16vmin; max-height: 16vmin;").col-shrink.on-left.animated.myanim.pulse
                span.break-word.col-shrink {{ nom_corpo }}
          transition(appear enter-active-class="animated bounceInRight")
            .text-center(key="h2" :style="font_sous_titre ? `font-family: ${font_sous_titre}` : ''" v-html="sous_titre").size-sous-titre
      //- Section présentation
      section.text-white
        .self-center.inside-section
          .text-center.size-titre(:style="`font-family: ${font_bouton}`") {{ bouton_presentation }}
          div(:style="`font-family: ${font_texte_normal}`" v-html="presentation").size-texte
      //- Section activités
      section.text-white
        .self-center.inside-section
          .text-center.size-titre(:style="`font-family: ${font_bouton};`") {{ bouton_activites }}
          vue-flux(:options="vfOptions" :images="activitesImages" :transitions="vfTransitions" :captions="activitesCaptions" :style="`font-family: ${font_texte_normal}`" ref="activites")
            template(v-slot:preloader)
              flux-preloader
            template(v-slot:caption)
              flux-caption(v-slot="captionProps")
                .flux-caption(v-html="captionProps.text")
            template(v-slot:controls)
              flux-controls
            template(v-slot:pagination)
              flux-pagination
      //- Section membres
      section.text-white
        .self-center.inside-section
          .text-center.size-titre(:style="`font-family: ${font_bouton}`") {{ bouton_membres }}
          q-carousel(v-model="slide"  style="height: 72vmin;" transition-prev="scale" transition-next="scale" infinite :autoplay="10000" swipeable animated control-color="white" navigation padding arrows :style="`font-family: ${font_texte_normal}`").rounded-borders
            q-carousel-slide(:name="`slide${index}`" v-for="[index, pack] of membres" :key="pack.key").flex.flex-center.justify-center.q-gutter-md
                q-card.my-card(v-for="membre of pack" :key="membre.id_perso.value").bg-black
                  q-img(:src="`https://imageserver.eveonline.com/character/${membre.id_perso.value}_256.jpg`")
                    .absolute-top.text-center {{ membre.nom_perso.value }}
                    .absolute-bottom.small-text {{ membre.role.value }}
                      br
                      | {{ membre.description_perso.value }}
      //- Section recrutement
      section.text-white
        .self-center.inside-section
          .text-center.size-titre(:style="`font-family: ${font_bouton}`")
            img(src="/statics/america-recrut.png").img-titre
            span.q-mx-lg {{ recrutement_titre }}
            img(src="/statics/america-recrut.png").img-titre
          q-img(:src="recrutement_image_de_fond" position="center center").qimg
            div(:style="`font-family: ${font_texte_normal};`" v-html="recrutement_descriptif").size-texte.absolute-full.text-subtitle2.flex.flex-center.column
      //- Section outils
      section.text-white
        .self-center.inside-section
          .text-center.size-titre(:style="`font-family: ${font_bouton}`") {{ bouton_outils }}
          vue-flux(:options="vfOptions" :images="outilsImages" :transitions="vfTransitions" :captions="outilsCaptions" :style="`font-family: ${font_texte_normal}`" ref="outils")
            template(v-slot:preloader)
              flux-preloader
            template(v-slot:caption)
              flux-caption(v-slot="captionProps")
                .flux-caption(v-html="captionProps.text")
            template(v-slot:controls)
              flux-controls
            template(v-slot:pagination)
              flux-pagination
    //- Bouton de copyright CCP
    q-btn(no-caps flat unelevated label="CCP Copyright Notice" @click="notice = true").copyright
    q-dialog(v-model="notice" style="position: fixed; top: 40%; left: 20%; width: 60%; height: 20%; z-index: 1;")
      q-card.black90.text-white
        q-card-section.row.items-center.q-ma-lg
          .text-h6 CCP Copyright Notice
          q-space
          q-btn(icon="close" flat round dense v-close-popup)
        q-card-section.row.q-ma-lg {{ noticeText }}
</template>

<style lang="stylus">
.dark-background
  position: absolute
  top: 0
  left: 0
  background-color: rgba(0, 0, 0, 0.60)
  width: 100%
  height: 100%
.video-background
  background: #000
  position: fixed
  top: 0
  right: 0
  bottom: 0
  left: 0
  z-index: -99
.video-foreground, .video-background iframe
  position: absolute
  top: 0
  left: 0
  width: 100%
  height: 100%
  pointer-events: none
@media (min-aspect-ratio: 16/9)
  .video-foreground
    height: 300%
    top: -100%
@media (max-aspect-ratio: 16/9)
  .video-foreground
    width: 300%
    left: -100%
.page-container
  position: absolute
  left: 0
  top: 0
  width: 100%
  height: 100%
#wrap
  position: relative
  overflow: hidden
.hidden-overflow
  overflow: hidden
section
  box-sizing: border-box
  position: relative
  height: 100vh
  overflow: hidden
  font-size: 10vmin
  padding: 1em
.inside-section
  margin-top: -50px
  background-color: rgba(0,0,0,0.5)
  padding: 1vh 1vw
.myanim
  animation-duration: 4s
  animation-delay: 2s
  animation-iteration-count: infinite
.size-titre
  font-size: 8vmin
.size-sous-titre
  font-size: 5vmin
.size-texte, .flux-caption-descriptif
  font-size: 2vmin
.img-titre
  height: 8vmin
.vue-flux, .qimg
  height: calc(72vh - 50px)
.flux-caption-titre
  font-size: 4vmin
  margin-bottom: 2vmin
.my-card
  font-size: 1.75vmin
  width: 26.5vmin
  height: 26.5vmin
.small-text
  font-size: 1.4vmin
.q-img__content > div
  padding: 8px 16px
a:link, a:visited, a:hover, a:active
  color: white
.break-word
  word-wrap: break-word
.copyright
  position: fixed
  right: 0
  bottom: 0
  text-align: center
  color: white
  opacity: 0.2
.copyright:hover
  opacity: 1
  background: transparent !important
  background-color: transparent !important
.black90
  background-color: rgba(0, 0, 0, 0.90)
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
    FluxPagination,
    FluxPreloader
  },
  data () {
    return {
      url: window.location.href,
      host: window.location.host,
      notice: false,
      FullPageScroll: FullPageScroll,
      slide: 'slide0',
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
      vfOptions: { autoplay: true, delay: 10000 },
      vfTransitions: [ 'concentric', 'warp', 'round1', 'round2', 'wave', 'waterfall', 'zip', 'blinds2d', 'blocks1', 'blocks2', 'blinds3d', 'explode' ],
      activitesImages: [],
      activitesCaptions: [],
      outilsImages: [],
      outilsCaptions: [],
      recrutement_titre: '',
      recrutement_descriptif: '',
      recrutement_image_de_fond: '',
      membres: new Map()
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
      for (const outil of items.outils.value) {
        this.outilsImages.push(outil.diapo__image_de_fond.value[0].url)
        this.outilsCaptions.push(`<div class="flux-caption-titre">${outil.diapo__titre.value}</div><div class="flux-caption-descriptif">${outil.diapo__descriptif.value}</div>`)
      }
      let i = 0
      for (const membre of items.membres.value) {
        const r = Math.trunc(i / 6)
        if (!this.membres.has(r)) this.membres.set(r, [])
        let tab = this.membres.get(r)
        tab.push(membre)
        this.membres.set(r, tab)
        i++
      }
    })
  },
  computed: {
    idYoutube () {
      if (this.id_youtube && this.player) return this.id_youtube
      else return ''
    },
    noticeText () { return `EVE Online and the EVE logo are the registered trademarks of CCP hf. All rights are reserved worldwide. All other trademarks are the property of their respective owners. EVE Online, the EVE logo, EVE and all associated logos and designs are the intellectual property of CCP hf. All artwork, screenshots, characters, vehicles, storylines, world facts or other recognizable features of the intellectual property relating to these trademarks are likewise the intellectual property of CCP hf. CCP hf. has granted permission to ${this.host} to use EVE Online and all associated logos and designs for promotional and information purposes on its website but does not endorse, and is not in any way affiliated with, ${this.host}. CCP is in no way responsible for the content on or functioning of this website, nor can it be liable for any damage arising from the use of this website.` }
  },
  mounted () {
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
