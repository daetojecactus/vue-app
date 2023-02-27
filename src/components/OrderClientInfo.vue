<template>

<ul class="dictionary">
    <li class="dictionary__item" v-for="item in dictionary" :key="item.id">
      <span class="dictionary__key">
        {{ item.key }}
      </span>
      <span class="dictionary__value">
        {{ item.value }}
      </span>
    </li>
  </ul>

</template>

<script>

export default {
  props: ['dictionary', 'loadOrderInfo'],
};

</script>
