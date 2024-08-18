<script setup>
import { inject } from 'vue'
import busketCard from './busket-card.vue'

const { cart, removeFromBusket } = inject('modal')
</script>

<template>
  <div class="flex flex-col flex-1">
    <busketCard
      @remove-from-busket="() => removeFromBusket(item)"
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
    />
  </div>
</template>
