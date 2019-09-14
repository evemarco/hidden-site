<template lang="pug">
  q-layout(view="lHh Lpr lFf")
    q-header(elevated style="background-color: rgba(0, 0, 0, 0.8)")
      q-toolbar(style="background-color: rgba(0, 0, 0, 0)")
        q-toolbar-title(style="font-family: 'Croissant One', cursive;") Hidden Baguette
        q-btn(flat).on-left Page 1
        q-btn(flat).on-left Page 2
        q-btn(flat).on-left Page 3
        q-btn(flat).on-left Page 4
        q-space
        q-btn(flat @click="openURL('https://forum.hidden-baguette.fr')").on-left Forum
        div(style="font-family: eve") Eve Online
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
      leftDrawerOpen: false
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
