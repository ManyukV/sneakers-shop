<script setup>
import Card from './card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavor'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :img-url="item.imageUrl"
      :price="item.price"
      :is-favor="item.isFavor"
      :on-click-favor="() => emit('addToFavor', item)"
    />
  </div>
</template>
