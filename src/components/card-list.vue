<script setup>
import Card from './card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['addToFavor', 'addToBusket'])
</script>

<template>
  <div v-auto-animate class="grid grid-cols-4 gap-5 mb-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :img-url="item.imageUrl"
      :price="item.price"
      :is-favor="item.isFavor"
      :on-click-favor="isFavorites ? null : () => emit('addToFavor', item)"
      :onClickAdd="() => emit('addToBusket', item)"
      :is-add="item.isAdd"
    />
  </div>
</template>
