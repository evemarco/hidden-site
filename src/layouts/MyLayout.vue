<template lang="pug">
  q-layout(view="lHh Lpr lFf")
    q-header(elevated style="background-color: rgba(0, 0, 0, 0.8)")
      q-toolbar(style="background-color: rgba(0, 0, 0, 0)")
        q-btn(flat no-caps dense :icon="`img:${image_logo}`" size="lg" style="font-family: 'Croissant One', cursive;" @click="$root.$emit('goToSlide', 0)" :label="nom_corpo")
        q-btn(flat no-caps dense @click="$root.$emit('goToSlide', 1)").on-right Page 1
        q-btn(flat no-caps dense @click="$root.$emit('goToSlide', 2)").on-right Page 2
        q-btn(flat no-caps dense @click="$root.$emit('goToSlide', 3)").on-right Page 3
        q-btn(flat no-caps dense @click="$root.$emit('goToSlide', 4)").on-right Page 4
        q-space
        q-btn(flat no-caps dense icon="img:https://image.flaticon.com/icons/svg/2111/2111370.svg" label="Discord" @click="openURL('https://discord.gg/cvunf46')").on-left
        q-btn(flat no-caps dense icon="chat" @click="openURL('https://forum.hidden-baguette.fr')" label="Forum").on-left
        q-btn(flat no-caps dense size="lg" @click="openURL('https://www.eveonline.com/fr/signup?invc=bf6cfbe6-d148-4ac6-ae98-cfd1494b1215')" style="font-family: eve" label="Eve Online")
    q-page-container
      router-view
</template>

<script>
import { openURL } from 'quasar'
import webFont from 'webfontloader'

export default {
  name: 'MyLayout',
  data () {
    return {
      leftDrawerOpen: false,
      nom_corpo: '',
      image_logo: ''
    }
  },
  created () {
    webFont.load({
      google: {
        families: ['Croissant One'],
        text: 'Hidden Baguette'
      }
    })
    this.$root.$on('site', (items) => {
      this.nom_corpo = items.nom_corpo.value
      this.image_logo = items.image_logo.value[0].url
    })
  },
  methods: {
    openURL
  }
}
</script>

<style lang="stylus">
@font-face
  font-family: eve
  src: url(../statics/eve.ttf)
</style>
