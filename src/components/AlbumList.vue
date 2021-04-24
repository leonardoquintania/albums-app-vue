<template>
  <h1>Album App</h1>
  <loader v-if="isLoading" />
  <div
    class="div-pokemon"
    v-for="(album, index) in list"
    @click="selectedAlbum(album.id)"
    :key="index"
  >
    <p>{{ album.title }}</p>
  </div>
</template>

<script>
import { getAlbumList } from '../services/album-service';
import Loader from './Loader';
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

export default {
  setup() {
    const list = ref([]);
    const router = useRouter();
    const isLoading = ref(false);

    onMounted(async () => {
      isLoading.value = true;
      const albums = await getAlbumList();
      list.value = albums;
      isLoading.value = false;
    });

    const selectedAlbum = (id) => {
      router.push({ name: 'album', params: { id } });
    };

    return { list, isLoading, selectedAlbum };
  },
  components: { Loader },
};
</script>

<style scoped>
.div-pokemon {
  display: flex;
  align-items: center;
  border-bottom: 1px solid lightgray;
  border-radius: 0.2rem;
  margin: 0.5rem;
  cursor: pointer;
}
</style>
