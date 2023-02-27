<template>
<!-- eslint-disable  -->
<div class="form__counter">
      <button type="button" aria-label="Убрать один товар" @click.prevent="inputNumber--">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>

      <input type="number" v-model.number="inputNumber" name="count" minlength="1">

      <button type="button" aria-label="Добавить один товар" @click.prevent="inputNumber++">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div>

</template>

<script>

export default {
  props: {
    value: Number,
  },
  computed: {
    inputNumber: {
      get() {
        return this.value;
      },
      set(value) {
        if (value > 0) {
          this.$emit('input', value);
        }
      },
    },
  },
};

</script>
