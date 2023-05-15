<template>
  <MainPage v-if="currentPage === 'main'" />
  <ProductPage v-else-if="currentPage === 'product'" />
</template>

<script>
import MainPage from '@/pages/MainPage.vue';
import ProductPage from '@/pages/ProductPage.vue';

export default {
  name: 'App',
  components: { MainPage, ProductPage },
  data() {
    return {
      currentPage: 'main',
    };
  },
};
</script>
