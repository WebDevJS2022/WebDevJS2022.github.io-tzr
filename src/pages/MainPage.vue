<template>
    <main class="content container">
   <div class="content__top content__top--catalog">
     <h1 class="content__title">
       Каталог
     </h1>
     <span class="content__info">
       152 товара
     </span>
   </div>

   <div class="content__catalog">

     <ProductFilter :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo"
     :category-id.sync="filterCategoryId" :color-id.sync="filterColorId" />

     <section class="catalog">

      <div v-if="productsLoading" class="loading">Загрузка товаров...
        <transition><img alt="preloader" src="/img/preloader.gif"></transition></div>

       <ProductList :products="products"/>
       <BasePagination v-model="page" :count="countProducts" :per-page="productsPerPage" />
     </section>

   </div>
 </main>
</template>

<script>
import ProductList from '@/components/ProductList.vue';
import BasePagination from '@/components/BasePagination.vue';
import ProductFilter from '@/components/ProductFilter.vue';
import axios from 'axios';

export default {
  name: 'MainPage',
  components: { ProductList, BasePagination, ProductFilter },
  data() {
    return {
      filterPriceFrom: null,
      filterPriceTo: null,
      filterCategoryId: 0,
      filterColorId: 0,
      page: 1,
      productsPerPage: 3,
      productsData: null, // Вывод списка товаров из API

      productsLoading: false, // обработка загрузки
    };
  },
  computed: {
    products() {
      return this.productsData
        ? this.productsData.items.map((product) => ({
          ...product,
          image: product.preview.file.url,
        }))
        : [];
    },
    countProducts() {
      return this.productsData ? this.productsData.pagination.total : 0; // пагинация из API
    },
  },
  //  Вывод списка товаров из API + пагинация
  methods: {
    loadProducts() {
      this.productsLoading = true; // в начале загрузки сообщаем что загрузка идет
      clearTimeout(this.loadProductsTimer);
      this.loadProductsTimer = setTimeout(() => {
        axios
          .get('https://vue-tzr.skillbox.cc/api/products', {
            params: {
              page: this.page, // номер страницы
              limit: this.productsPerPage, // количество элементов
              categoryId: this.filterCategoryId,
              minPrice: this.filterPriceFrom,
              maxPrice: this.filterPriceTo,
              colorId: this.filterColorId,
            },
          })
          .then((response) => { this.productsData = response.data; })
          // когда загрузка произошла и данные получены, убираем свойство productsLoading
          .then(() => { this.productsLoading = false; });
      }, 500);
    },
  },
  // пагинация из API
  watch: {
    page() {
      this.loadProducts();
    },
    filterPriceFrom() {
      this.loadProducts();
    },
    filterPriceTo() {
      this.loadProducts();
    },
    filterCategoryId() {
      this.loadProducts();
    },
    filterColorId() {
      this.loadProducts();
    },
  },
  created() {
    this.loadProducts();
  },
};
</script>

<style>
.loading {
  margin: auto;
}
</style>
