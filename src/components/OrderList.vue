<template>
      <!-- eslint-disable  -->
  <div class="cart__block">
    <ul class="cart__orders">
      <li class="cart__order" v-for="item in products" :key="item.productId">
        <h3>{{ item.product.title }}</h3>
        <b>{{ (item.product.price * item.amount) | numberFormat }} ₽</b>
        <span>Артикул: {{ item.productId }}</span>
      </li>
    </ul>

    <div class="cart__total">
      <p>Доставка: <b>500 ₽</b></p>
      <p>Итого: <b>{{ products.length }}</b> товара на сумму <b>{{ totalPrice | numberFormat }} ₽</b></p>
    </div>

    <button class="cart__button button button--primery" type="submit" v-if="showButton">
      Оформить заказ
    </button>
  </div>
</template>

<script>

import numberFormat from '@/helpers/numberFormat';

export default {
  props: ['products', 'totalPrice', 'loading', 'showButton'],
  filters: {
    numberFormat,
  },
};
</script>
