<template lang="pug">
  q-layout(view="lHh Lpr lFf")
    q-header(elevated).qheader
      q-toolbar
        q-btn(flat no-caps dense :icon="image_logo ? `img:${image_logo}` : ''" size="lg" :style="`font-family: ${font_logo}, cursive;`" @click="$root.$emit('goToSlide', 0)" :label="nom_corpo")
          q-spinner(v-if="!image_logo" color="primary")
        q-btn(flat no-caps dense :style="`font-family: ${font_bouton}`" @click="$root.$emit('goToSlide', 1)" :label="bouton_presentation").on-right
        q-btn(flat no-caps dense :style="`font-family: ${font_bouton}`" @click="$root.$emit('goToSlide', 2)" :label="bouton_activites").on-right
        q-btn(flat no-caps dense :style="`font-family: ${font_bouton}`" @click="$root.$emit('goToSlide', 3)" :label="bouton_membres").on-right
        q-btn(flat no-caps dense :style="`font-family: ${font_bouton}`" @click="$root.$emit('goToSlide', 4)" :label="bouton_recrutement").on-right
        q-btn(flat no-caps dense :style="`font-family: ${font_bouton}`" @click="$root.$emit('goToSlide', 5)" :label="bouton_outils").on-right
        q-space
        q-btn(v-if="lien_discord" flat no-caps dense :style="`font-family: ${font_bouton}`" icon="img:/statics/discord.svg" label="Discord" @click="openURL(lien_discord)").on-left
        q-btn(v-if="lien_forum" flat no-caps dense :style="`font-family: ${font_bouton}`" icon="chat" @click="openURL(lien_forum)" label="Forum").on-left
        q-btn(v-if="lien_eve_online" flat no-caps dense size="lg" @click="openURL(lien_eve_online)" style="font-family: eve" label="Eve Online")
    q-page-container
      router-view
</template>

<script>
import { openURL } from 'quasar'

export default {
  name: 'MyLayout',
  meta () {
    return {
      title: this.meta_title,
      meta: {
        description: { name: 'description', content: this.meta_description }
      }
    }
  },
  data () {
    return {
      nom_corpo: '',
      image_logo: '',
      bouton_presentation: '',
      bouton_activites: '',
      bouton_membres: '',
      bouton_recrutement: '',
      bouton_outils: '',
      lien_discord: '',
      lien_forum: '',
      lien_eve_online: '',
      font_logo: '',
      font_bouton: '',
      meta_title: '',
      meta_description: ''
    }
  },
  created () {
    this.$root.$on('site', (items) => {
      this.nom_corpo = items.nom_corpo.value
      this.image_logo = items.image_logo.value[0].url
      this.bouton_presentation = items.bouton_presentation.value
      this.bouton_activites = items.bouton_activites.value
      this.bouton_membres = items.bouton_membres.value
      this.bouton_recrutement = items.bouton_recrutement.value
      this.bouton_outils = items.bouton_outils.value
      this.lien_discord = items.lien_discord.value
      this.lien_forum = items.lien_forum.value
      this.lien_eve_online = items.lien_eve_online.value
      this.font_logo = items.font_logo.value
      this.font_bouton = items.font_bouton.value
      this.meta_title = items.meta_title.value
      this.meta_description = items.meta_description.value
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
.qheader
  background-color: rgba(0, 0, 0, 0.8)
</style>
