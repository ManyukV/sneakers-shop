<script setup>
import CardList from '../components/card-list.vue'
import { onMounted, ref } from 'vue'
import axios from 'axios'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://aa61e44bd2676303.mokky.dev/favorites?_relations=items'
    )
    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <div class="p-10">
    <CardList :items="favorites" is-favorites />
  </div>
</template>
