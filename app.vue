<script setup>
import {ref, computed} from 'vue'
import HeaderComponent from "~/components/HeaderComponent.vue";

  let photoGallery = ref([])

  const numberOfPhoto = computed(() => photoGallery.value.length)
  const evenAlbums = computed(() => photoGallery.value.filter((photo) => photo?.albumId % 2 === 0).length)
  const oddAlbums = computed(() => photoGallery.value.filter((photo) => !(photo?.albumId % 2 === 0)).length)


  const fetchPhotos = async () => {
    const resp = await fetch('https://jsonplaceholder.typicode.com/photos')
    photoGallery.value = await resp.json()
  }

</script>

<template>
  <div>
    <HeaderComponent>
      <template #header>
        <h1>Title for header component</h1>
      </template>

      <template #body>
        <h1>Somebody for header component</h1>
      </template>
    </HeaderComponent>

    {{ numberOfPhoto }} photos ({{ oddAlbums }} odd albums | {{ evenAlbums }} event albums )

    <button @click="fetchPhotos">Fetch photos</button>
    <ul :class="$style.todoList">
      <li v-for="photo in photoGallery">
        <img :src="photo?.thumbnailUrl" :key="`photo-id-${photo?.id}`" alt="photo">
      </li>
    </ul>
  </div>
</template>

<style module>
  .todoList {
    text-decoration: none;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
  }
</style>
