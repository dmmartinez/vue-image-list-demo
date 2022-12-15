<template>
  <v-flex grow shrink>
    <images-component :items-load="20" />
  </v-flex>
</template>

<script lang="ts">
  import { defineComponent } from 'vue'
  import ImagesComponent from '~/components/ImagesComponent.vue'
  export default defineComponent({
    name: 'IndexPage',
    components: { ImagesComponent },
  })
</script>
