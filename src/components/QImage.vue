<template>
  <div
    class="q-image"
    :class="{
      loading: lazyload,
      loaded: !lazyload
    }"
    :style="{ backgroundColor: color }">

    <picture
      class="image"
      ref="image">
      <source
        v-if="jpg"
        type="image/jpg"
        :data-srcset="lazyload && jpgSrcSet"
        :srcset="!lazyload && jpgSrcSet"
        :sizes="sizes">
      <source
        v-if="webp"
        type="image/webp"
        :data-srcset="lazyload && webpSrcSet"
        :srcset="!lazyload && webpSrcSet"
        :sizes="sizes">
      <img
        ref="baseimage"
        :data-src="lazyload && src"
        :src="!lazyload && src"
        :alt="alt"
        :style="{
          objectFit: placement,
          objectPosition: position
        }">
    </picture>
  </div>
</template>

<script>
// utils
import { isMinLength } from '../validators.js'

export default {
  props: {
    // basic props
    alt: {
      type: String,
      required: true,
      default: 'Image non disponible',
      validator: value => isMinLength(value, 5)
    },
    src: {
      type: String,
      required: true
    },
    // repsonsive images
    jpg: {
      type: Array,
      required: false,
      default: null
    },
    webp: {
      type: Array,
      required: false,
      default: null
    },
    sizes: {
      type: String,
      required: false,
      default: null
    },
    // style props
    placement: {
      type: String,
      required: false,
      default: () => 'cover',
      validator: (value) => ['contain', 'cover', 'none'].indexOf(value) > -1
    },
    position: {
      type: String,
      required: false,
      default: () => 'center'
    },
    color: {
      type: String,
      required: false,
      default: 'none'
    },
    // lazyload props
    lazyload: {
      type: Boolean,
      required: false,
      default: false
    },
    lazyloadType: {
      type: String,
      required: false,
      default: 'direct',
      validator: (value) => ['direct', 'intersection', 'called'].indexOf(value) > -1
    }
  },
  data () {
    return {
      loaded: false
    }
  },
  computed: {
    jpgSrcSet () {
      if (!this.jpg) return ''
      return this.jpg
        .map(jpgSource => `${jpgSource.source} ${jpgSource.width}w`)
        .join(', ')
    },
    webpSrcSet () {
      if (!this.webp) return ''
      return this.webp
        .map(webpSource => `${webpSource.source} ${webpSource.width}w`)
        .join(', ')
    }
  },
  mounted () {
    if (this.lazyload && this.lazyloadType === 'direct') {
      this.lazyloadImage()
    }

    if (this.$refs.baseimage.src && this.$refs.baseimage.complete) {
      this.onLoaded()
    } else {
      this.$refs.image.addEventListener('load', this.onLoaded)
    }
  },
  methods: {
    lazyloadImage () {
      Array.from(this.$refs.image.querySelectorAll('source'))
        .forEach(source => {
          source.srcset = source.dataset.srcset
        })
      this.$refs.baseimage.src = this.$refs.baseimage.dataset.src
    },
    onIntersect () {
      this.lazyloadImage()
    },
    onLoaded () {
      this.$emit('loaded', this)
      this.loaded = true
      this.$el.classList.remove('loading')
    }
  }
}
</script>

<style lang="stylus" scoped>
  .q-image
    position relative
    overflow hidden
    transition opacity 0.3s
    width 100%
    height 100%

    &.loading
      opacity 0

    &.loaded
      opacity 1

  .image
    display inline-block
    width 100%
    height 100%


  .image img
    width 100%
    height 100%
    object-fit cover
    user-select none
</style>
