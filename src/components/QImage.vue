<template>
  <div
    class="q-image"
    :class="{
      loading: useLazyload,
      loaded: !useLazyload
    }"
    :style="{ backgroundColor: color }">

    <picture
      class="image"
      ref="image">
      <source
        v-if="jpg"
        type="image/jpg"
        :data-srcset="useLazyload && jpgSrcSet"
        :srcset="!useLazyload && jpgSrcSet"
        :sizes="sizes">
      <source
        v-if="webp"
        type="image/webp"
        :data-srcset="useLazyload && webpSrcSet"
        :srcset="!useLazyload && webpSrcSet"
        :sizes="sizes">
      <img
        ref="baseimage"
        :data-src="useLazyload && source"
        :src="!useLazyload && source"
        :alt="alt"
        :style="{
          objectFit: placement,
          objectPosition: position
        }">
    </picture>
  </div>
</template>

<script>
export default {
  props: {
    // basic props
    alt: {
      type: String,
      required: true,
      default: 'Image non disponible'
    },
    source: {
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
      default: '100vw'
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
    // lazyload
    lazyload: {
      type: String,
      required: false,
      default: 'none',
      validator: (value) => ['none', 'direct', 'intersection', 'called'].indexOf(value) > -1
    },
    threshold: {
      type: Number,
      required: false,
      default: 0.0
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
    },
    useLazyload () {
      return this.lazyload !== 'none'
    }
  },
  mounted () {
    if (this.lazyload === 'direct') {
      this.lazyloadImage()
    } else if (this.lazyload === 'intersection') {
      this.setIntersectionObserver()
    }

    if (this.$refs.baseimage.src && this.$refs.baseimage.complete) {
      this.onLoaded()
    } else {
      this.$refs.baseimage.addEventListener('load', this.onLoaded)
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
    onLoaded () {
      this.$emit('loaded', this.$refs.image)
      this.loaded = true
      this.$el.classList.remove('loading')
    },
    // Intersection Observer
    setIntersectionObserver () {
      if (!this.supportIntersectionObserver()) {
        console.warn('IntersectionObserver not supported, loading image now')
        this.lazyloadImage()
      }

      let observer = new IntersectionObserver(intersections => {
        if (intersections[0].isIntersecting) {
          this.lazyloadImage()
        }
      }, {
        threshold: this.threshold
      })

      observer.observe(this.$el)
    },
    supportIntersectionObserver () {
      return 'IntersectionObserver' in window && 'IntersectionObserverEntry' in window && 'intersectionRatio' in window.IntersectionObserverEntry.prototype
    }
  }
}
</script>

<style scoped>
  .q-image {
    position: relative;
    overflow: hidden;
  }

  .image {
    display: inline-block;
    width: 100%;
    height: 100%;
  }

  .image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    user-select: none;
  }
</style>
