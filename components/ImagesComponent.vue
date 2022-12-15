<template>
  <v-layout row wrap>
    <v-progress-circular v-if="loading" indeterminate color="primary" />
    <div v-if="!loading" class="box-length pa-2">Images length: {{ imagesView.length }}</div>
    <div v-if="!loading" class="imgs-container">
      <div v-for="(image, index) in imagesView" :key="image.id" class="img-container pl-2 pr-2" @click="removeImage(index)">
        <image-component :url="image.url" :title="image.title" />
      </div>
    </div>
    <div v-intersect="loadNext" class="pa-1"></div>
    <v-progress-circular v-if="loading" indeterminate color="primary" />
  </v-layout>
</template>
<script lang="ts">
  import Vue from 'vue'
  import axios from 'axios'
  import ImageComponent from '~/components/ImageComponent.vue'

  export default Vue.extend({
    name: 'ImagesComponent',
    components: { ImageComponent },
    props: {
      itemsLoad: {
        type: Number,
        default: 20,
        required: false
      }
    },
    data() {
      return {
        loading: true,
        imagesView: [],
        images: [],
      }
    },
    computed: {
      imagesUrl() {
        return 'https://jsonplaceholder.typicode.com/photos'
      },
      indexIntersect() {
        if (this.loading) {
          return 0
        } else {
          return Math.floor(this.imagesView.length - (this.imagesView.length / 3))
        }
      },
    },
    async created() {
      this.loading = true
      const res = await axios.get(this.imagesUrl)
      this.images = res.data
      this.imagesView = this.images.slice(0, this.itemsLoad)
      this.images.splice(0, this.itemsLoad)
    },
    mounted() {
      this.loading = false
    },
    methods: {
      loadNext(_entries, _observer, isIntersecting) {
        if (isIntersecting) {
          setTimeout(() => {
            this.loading = true
            if (!!this.images && !!this.images.length) {
              this.imagesView.push(...this.images.slice(0, this.itemsLoad))
              this.images.splice(0, this.itemsLoad)
            }
            this.loading = false
          })
        }
      },
      removeImage(index) {
        this.imagesView.splice(index, 1)
      },
    },
  })
</script>
<style>
  .imgs-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    width: 100%;
  }
  .box-length {
    position: fixed;
    background: black;
    color: white;
  }
  @media screen and (max-width: 600px) {
    .img-container {
      width: 100%;
    }
  }
  @media screen and (min-width: 601px) and (max-width: 960px) {
    .img-container {
      width: 50%;
    }
  }
  @media screen and (min-width: 961px) and (max-width: 1264px) {
    .img-container {
      width: 33%;
    }
  }
  @media screen and (min-width: 1265px) {
    .img-container {
      width: 25%;
    }
  }
</style>
