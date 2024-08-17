<script setup>
import Card from './card.vue'

defineProps({
  items: Array
})

function onClickAdd() {
  alert('Добавленно в корзину')
}

function onClickFavor() {
  alert('Добавленно в избранное')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :img-url="item.imageUrl"
      :price="item.price"
      :on-click-add="onClickAdd"
    />
  </div>
</template>
