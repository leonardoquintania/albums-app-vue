<template>
  <Button label="Voltar" @click="back" />
  <loader v-if="isLoading" />
  <div class="div-row" v-for="(photo, index) in list" :key="index">
    <img :src="photo.thumbnailUrl" :alt="photo.id" />
    <p>{{ photo.title }}</p>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import Loader from './Loader';
import { getPhotos } from '../services/album-service.js';

export default {
  components: { Loader },
  setup() {
    const router = useRouter();
    const route = useRoute();
    const { id } = route.params;
    const list = ref([]);
    const isLoading = ref(false);

    onMounted(async () => {
      isLoading.value = true;
      const photos = await getPhotos(id);
      console.log(photos);
      list.value = photos;
      isLoading.value = false;
    });

    const back = () => router.replace('/');

    return { router, list, isLoading, back };
  },
};
</script>

<style scoped>
.div-row {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 1rem;
  border-bottom: 1px solid lightgray;
}
</style>
