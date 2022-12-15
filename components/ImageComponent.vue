<template>
  <img :src="url" :alt="title" class="im pa-1" />
</template>
<script>
  export default {
    name: 'ImageComponent',
    props: {
      url: {
        type: String,
        required: true,
      },
      title: {
        type: String,
        required: true,
      },
    },
  }
</script>
<style>
  img.im {
    width: 100%;
  }
</style>
