<template><!-- eslint-disable  -->
  <li class="catalog__item">
    <router-link class="catalog__pic" :to="{name: 'product', params: {id: product.id}}">
      <img :src="product.image" :alt="product.title">
    </router-link>

    <h3 class="catalog__title">
      <a href="#">
        {{ product.title }}
      </a>
    </h3>

    <span class="catalog__price">
      {{ product.price | numberFormat }} ₽
    </span>

    <ColorsList class="colors--black" :colors="colors" :elementName="'product_' + product.id" :current-color="firstColor"/>

</li>
</template>

<script>
/* eslint-disable */

import gotoPage from '@/helpers/gotoPage';
import numberFormat from '@/helpers/numberFormat';
import ColorsList from '@/components/ColorsList.vue';

export default {
  data() {
    return {
      color: '#73B6EA'
    };
  },
  components: {
    ColorsList,
  },
  filters: {
    numberFormat
  },
  methods: {
    gotoPage
  },
  props: ['product'],
  computed: {
    colors() {
      return this.product.colors;
    },
    firstColor() {
      return this.colors[0].id;
    },
  },
};

</script>
