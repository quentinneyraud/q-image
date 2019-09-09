# Introduction

Vue component for images using picture tag, srcset and lazyload

## Install

```bash
yarn add @qneyraud/q-image
```

## Usage

```js
import QImage from '@qneyraud/q-image'
import '@qneyraud/q-image/dist/QImage.css'

// or
import QImage from '@qneyraud/q-image/src/components/QImage'


export default {
  components: {
    QImage
  }
}
```

## Development

```
# clone repo
git clone git@github.com:quentinneyraud/q-image.git
cd q-image

# install dependencies
yarn

# Run example
npm run serve
```

## Build

```bash
npm run build-bundle
```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# Options

## alt

> Alternative text 

**Type**: String   
**Required**: true   
**Default**: '' 

## source

> Default image source

**Type**: String   
**Required**: true   
**Default**: '' 

## jpg

> List of JPG images with `source` and `width` properties

**Type**: Array   
**Required**: false   
**Default**: null

## webp

> List of WEBP images with `source` and `width` properties

**Type**: Array   
**Required**: false   
**Default**: null

## sizes

> Image tag `sizes` attribute

**Type**: String   
**Required**: false   
**Default**: null 

## placement

> `object-fit` css rule      

**Type**: String   
**Required**: false   
**Default**: 'cover' 

## position

> `object-position` css rule      

**Type**: String   
**Required**: false   
**Default**: 'center center' 

## color

> Component background color. You will see it before image lazyloading or if placement is 'contain'   

**Type**: String   
**Required**: false   
**Default**: null

## lazyload

> `none` to disable lazyloading  
> `direct` to lazyload image after page load  
> `called` to wait for a `lazyloadImage()` method call  
> `intersection` to use IntersectionObserver    

**Type**: String   
**Required**: false   
**Default**: 'none' 

## threshold

> IntersectionObserver threshold if lazyload is `intersection`

**Type**: Number   
**Required**: false   
**Default**: 0.0

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# Methods

## lazyloadImage

> Start lazyloading the image

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# Events

## loaded

> Emitted when the image is loaded

**Parameter**: reference to the component

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# Examples

## Simple image

```vue
<q-image 
  alt="my image"
  source="image.jpg"/>
```

## Simple image with custom styles

```vue
<template>
  <q-image 
    class="image"
    alt="my image"
    source="image.jpg"/>
</template>

<style>
  .image 
    transition: opacity 0.3s;

  .image.loading
    opacity: 0;

  .image.loaded
    opacity: 1;

</style>
```

## Responsive image

```vue
<q-image 
  alt="my image"
  source="image_900.jpg"
  jpg="[{
    source: 'image_450.jpg',
    width: 450
  }, {
    source: 'image_900.jpg',
    width: 900
  }, {
    source: 'image_1440.jpg',
    width: 1440
  }]"/>
```

## Responsive image with WEBP

```vue
<q-image 
  alt="my image"
  source="http://placecorgi.com/900/180"
  jpg="[{
    source: 'image_450.jpg',
    width: 450
  }, {
    source: 'image_900.jpg',
    width: 900
  }, {
    source: 'image_1440.jpg',
    width: 1440
  }]"
  webp="[{
    source: 'image_450.webp',
    width: 450
  }, {
    source: 'image_900.webp',
    width: 900
  }, {
    source: 'image_1440.webp',
    width: 1440
  }]"/>
```

## Image loaded on intersection

> Start loading when half of the image is visible

```vue
<q-image 
  alt="my image"
  source="image.jpg"
  lazyload="intersection"
  threshold="0.5"/>
```

## Image loaded by lazyloadImage method

```vue
<template>
  <q-image 
    ref="image"
    alt="my image"
    source="image.jpg"
    lazyload="called"/>
</template>

<script>
export default {
  methods: {
    displayImage () {
      this.$refs.image.lazyloadImage()
    }
  }
}
</script>

```

## Listen to loaded event 

```vue
<template>
  <q-image 
    @loaded="onImageLoaded"
    ref="image"
    alt="my image"
    source="image.jpg"
    lazyload="intersection"/>
</template>

<script>
export default {
  methods: {
    onImageLoaded (el) {
      console.log(el + ' is loaded')
    }
  }
}
</script>

```
