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
  data () {
    return {
      image: {
        alt: 'Une image d\'illustration',
        src: require('./images/landscape.jpg'),
        color: 'red',
        placement: 'cover',
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
        sizes: '(min-width: 1200px) 75vw, 100vw'
      }
    }
  },
  components: {
    QImage
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
  width: 100vw;
  height: 500px;

  @media screen and (min-width: 1200px) {
    display block
    margin 0 auto
    width 75vw
  }
</style>
