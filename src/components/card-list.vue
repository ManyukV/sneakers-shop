<script setup>
import Card from './card.vue'

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
      img-url="/sneakers/sneakers-1.jpg"
      title="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="1200"
      :is-add="false"
      :is-favor="false"
      :on-click-add="onClickAdd"
      :on-click-favor="onClickFavor"
    />
  </div>
</template>
