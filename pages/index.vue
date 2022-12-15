<template>
  <v-flex grow shrink>
    <images-component :items-load="20" />
  </v-flex>
</template>

<script lang="ts">
  import Vue from 'vue'
  import ImagesComponent from '~/components/ImagesComponent.vue'
  export default Vue.extend({
    name: 'IndexPage',
    components: { ImagesComponent },
  })
</script>
