<template> <!-- eslint-disable  -->
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

      <ProductFilter :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo" :category-id.sync="filterCategoryId" :product-color.sync="filterColorId"/>

      <section class="catalog">

        <figure v-if="productsLoading" style="display: flex;">
                <img width="25px" height="25px" src="/img/loading.gif"/>
                <figcaption>Загрузка товаров...</figcaption>
            </figure>
        <div v-if="productsLoadingFailed">Произошла ошибка при загрузке товаров
          <button @click.prevent="loadProducts">Попробовать еще раз</button>
        </div>

        <ProductList :products="products"/>

        <BasePagination v-model="page" :count="countProducts" :per-page="productsPerPage" />

      </section>

    </div>
</main>
</template>

<script>
/* eslint-disable */
import ProductList from '@/components/ProductList.vue';
import BasePagination from '@/components/BasePagination.vue';
import ProductFilter from '@/components/ProductFilter.vue';
import axios from 'axios';
import {API_BASE_URL} from '@/config.js';

export default {
  components: { ProductList, BasePagination, ProductFilter },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryId: 0,
      filterColorId: 0,

      page: 1,
      productsPerPage: 3,
      productsData: null,
      productsLoading: false,
      productsLoadingFailed: false,
    };
  },
  computed: {

    products() {
     return this.productsData
      ? this.productsData.items.map(product => {
        return {
          ...product,
          image: product.image.file.url
        }
      })
       : [];
    },
    countProducts() {
      return this.productsData ? this.productsData.pagination.total : 0;
    },
  },
  methods: {
    loadProducts() {
      this.productsLoading = true; //показывает надпись "загрузка товаров" при ожидании
      this.productsLoadingFailed = false; //показывает, что произошла ошибка
      clearTimeout(this.loadProductsTimer); // для того что бы отправлялся только один запрос
      this.loadProductsTimer = setTimeout(() => {
        axios.get(API_BASE_URL + '/api/products', {
        params: {
          page: this.page,
          limit: this.productsPerPage,
          categoryId: this.filterCategoryId,
          minPrice: this.filterPriceFrom,
          maxPrice: this.filterPriceTo,
          colorId: this.filterColorId,
        }
      })
      .then(response => this.productsData = response.data)
      .catch(() => this.productsLoadingFailed = true)
      .then(() => this.productsLoading = false);
      }, 0);
    },
  },
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
