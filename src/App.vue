<template>
  <div id="app">
    <q-image
      class="app-image"
      v-bind="image"/>
  </div>
</template>

<script>
import QImage from './components/QImage.vue'

export default {
  name: 'app',
  components: {
    QImage
  },
  data () {
    return {
      image: {
        // alternative text
        alt: 'Une image d\'illustration',
        // default image source
        source: require('./images/landscape.jpg'),
        // background color. You will see it before lazyloading or if placement is 'contain'
        color: 'red',
        // object-fit css rule
        placement: 'cover',
        // object-position css rule
        position: 'center center',
        // List of jpg files with their width
        jpg: [{
          source: require('./images/landscape_650.jpg'),
          width: 650
        }, {
          source: require('./images/landscape_900.jpg'),
          width: 900
        }, {
          source: require('./images/landscape_1400.jpg'),
          width: 1400
        }],
        // List of jpg files with their width
        webp: null,
        // img src-set sizes attribute
        sizes: '(min-width: 1200px) 75vw, 100vw',
        //
        lazyload: 'intersection',
        threshold: 0.75
      }
    }
  }
}
</script>

<style lang="stylus">
  html, body
    margin 0
    padding 0
</style>

<style lang="stylus" scoped>
.app-image
  width 100vw
  height 500px
  margin-top 2000px

  @media screen and (min-width: 1200px) {
    display block
    margin 0 auto
    margin-top 2000px
    width 75vw
  }
</style>
