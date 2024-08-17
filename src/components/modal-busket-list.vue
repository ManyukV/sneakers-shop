<script setup>
import { inject } from 'vue'
import busketCard from './busket-card.vue'

const { cart } = inject('modal')
</script>

<template>
  <div class="flex flex-col flex-1">
    <busketCard
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
    />
  </div>
</template>
