<template>
  <!-- eslint-disable  -->
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{name: 'main'}">
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{name: 'cart'}">
            Корзина
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Оформление заказа
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Корзина
      </h1>
      <span class="content__info">
        {{ $store.state.cartProducts.length }} товара
      </span>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST" @submit.prevent="order">
        <div class="cart__field">
          <div class="cart__data">

            <BaseFormText v-model="formData.name" :error="formError.name" title="ФИО" type="text" placeholder="Введите ваше полное имя"/>
            <BaseFormText v-model="formData.address" :error="formError.address" title="Адрес доставки" type="text" placeholder="Введите ваш адрес"/>
            <BaseFormText v-model="formData.phone" :error="formError.phone" title="Телефон" type="tel" placeholder="Введите ваш телефон"/>
            <BaseFormText v-model="formData.email" :error="formError.email" title="Email" type="email" placeholder="Введи ваш Email"/>
            <BaseFormTextarea title="Комментарий к заказу" :error="formError.comment" v-model="formData.comment" placeholder="Ваши пожелания"/>

          </div>

          <div class="cart__options">
            <h3 class="cart__title">Доставка</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="delivery" value="0">
                  <span class="options__value">
                    Самовывоз <b>бесплатно</b>
                  </span>
                </label>
              </li>
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="delivery" value="500">
                  <span class="options__value">
                    Курьером <b>500 ₽</b>
                  </span>
                </label>
              </li>
            </ul>

            <h3 class="cart__title">Оплата</h3>
            <ul class="cart__options options">
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="pay" value="card">
                  <span class="options__value">
                    Картой при получении
                  </span>
                </label>
              </li>
              <li class="options__item">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="pay" value="cash">
                  <span class="options__value">
                    Наличными при получении
                  </span>
                </label>
              </li>
            </ul>
          </div>
        </div>

        <OrderList :products="products" :totalPrice="totalPrice" :amount="$store.state.cartProducts.length" :showButton="true"/>

        <div class="cart__error form__error-block" v-if="formErrorMessage">
          <h4>Заявка не отправлена!</h4>
          <p>
            {{ formErrorMessage }}
          </p>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
/* eslint-disable */
import BaseFormText from '@/components/BaseFormText.vue';
import BaseFormTextarea from '@/components/BaseFormTextarea.vue';
import OrderList from '@/components/OrderList.vue';
import axios from 'axios';
import { mapGetters } from 'vuex';
import { API_BASE_URL } from '@/config';

export default {
  components: { BaseFormText, BaseFormTextarea, OrderList },
  data() {
    return {
      formData: {},
      formError: {},
      formErrorMessage: '',
    };
  },
  computed: {
    ...mapGetters({ products: 'cartDetailProducts', totalPrice: 'cartTotalPrice' }),
  },
  methods: {
    order() {
      this.formError = {};
      this.formErrorMessage = '';
      axios.post(API_BASE_URL + '/api/orders', {
        ...this.formData,
      }, {
        params: {
          userAccessKey: this.$store.state.userAccessKey,
        },
      })
      .then(response => {
        this.$store.commit('resetCart');
        this.$store.commit('updateOrderInfo', response.data);
        this.$router.push({name: 'orderInfo', params: {id: response.data.id}});
      })
      .catch(error => {
        this.formError = error.response.data.error.request || {};
        this.formErrorMessage = error.response.data.error.message;
      })
    },
  },
};

</script>
