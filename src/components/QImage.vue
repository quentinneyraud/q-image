<template>
  <div class="image">
    <img :src="src" alt="">
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      required: true
    }
  }
}
</script>
