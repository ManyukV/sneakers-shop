<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import Header from './components/header.vue'
import cardList from './components/card-list.vue'
import Banner from './components/banner.vue'

const items = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://aa61e44bd2676303.mokky.dev/items')
    items.value = data
  } catch (e) {
    console.log(err)
  }
})
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />
    <Banner />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-md outline-none">
            <option>По названию</option>
            <option>По цене(по убыванию)</option>
            <option>По цене(по возрастанию)</option>
          </select>

          <div class="relative">
            <img class="absolute left-3 top-3" src="/search.svg" alt="search" />
            <input
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск"
            />
          </div>
        </div>
      </div>
      <cardList :items="items" />
    </div>
  </div>
</template>

<style scoped></style>
