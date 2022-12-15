<template>
  <v-layout row wrap>
    <v-progress-circular v-if="loading" indeterminate color="primary" />
    <div v-if="!loading" class="box-length pa-2">Images length: {{ imagesView.length }}</div>
    <div v-if="!loading" class="imgs-container">
      <div v-for="(image, index) in imagesView" :key="image.id" class="img-container pl-2 pr-2" @click="removeImage(index)">
        <image-component :url="image.url" :title="image.title" />
        <div v-if="!!indexIntersect && (index == indexIntersect)" v-intersect="loadNext" class="pa-1"></div>
      </div>
    </div>
    <div v-intersect="loadNext" class="pa-1"></div>
    <v-progress-circular v-if="loading" indeterminate color="primary" />
  </v-layout>
</template>
<script lang="ts">
  import { defineComponent } from 'vue'
  import axios from 'axios'
  import ImageComponent from '~/components/ImageComponent.vue'

  export default defineComponent({
    name: 'ImagesComponent',
    components: { ImageComponent },
    props: {
      itemsLoad: {
        type: Number,
        default: 20,
        required: false,
      },
      loadMoreRemaining: {
        type: Number,
        default: 5,
        required: false,
      },
    },
    data() {
      return {
        loading: true,
        imagesView: [],
        images: [],
      }
    },
    computed: {
      imagesUrl(): string {
        return 'https://jsonplaceholder.typicode.com/photos'
      },
      indexIntersect(): number {
        if (this.imagesView.length > this.loadMoreRemaining) {
          return this.imagesView.length - this.loadMoreRemaining
        }
        return 0
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
      loadNext(_entries: IntersectionObserverEntry[],
               _observer: IntersectionObserver,
               isIntersecting: boolean) {
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
      removeImage(index: number) {
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
      width: 50%;
    }
  }
  @media screen and (min-width: 601px) and (max-width: 960px) {
    .img-container {
      width: 33%;
    }
  }
  @media screen and (min-width: 961px) {
    .img-container {
      width: 25%;
    }
  }
</style>
