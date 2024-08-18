<script setup>
import { watch, reactive, inject, ref, onMounted } from 'vue'

import axios from 'axios'
import cardList from '../components/card-list.vue'
import Banner from '../components/banner.vue'

const { cart, addToBusket, removeFromBusket } = inject('modal')
const items = ref([])

function onChangeSelect(event) {
  filters.sortBy = event.target.value
}

async function fetchItems() {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.search) {
      params.title = `*${filters.search}*`
    }

    const { data } = await axios.get(`https://aa61e44bd2676303.mokky.dev/items`, {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavor: false,
      favoriteId: null,
      isAdd: false
    }))
  } catch (e) {
    console.log(e)
  }
}

function onChangeInput(event) {
  filters.search = event.target.value
}

function busketChange(item) {
  if (!item.isAdd) {
    addToBusket(item)
  } else {
    removeFromBusket(item)
  }
}

const filters = reactive({
  sortBy: 'title',
  search: ''
})

async function addToFavor(item) {
  try {
    if (!item.isFavor) {
      const obj = {
        item_id: item.id,
        item
      }
      item.isFavor = true
      const { data } = await axios.post(`https://aa61e44bd2676303.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    } else {
      item.isFavor = false
      await axios.delete(`https://aa61e44bd2676303.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

async function fetchFavor() {
  try {
    const { data: favorites } = await axios.get(`https://aa61e44bd2676303.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!favorite) {
        return item
      }

      return { ...item, isFavor: true, favoriteId: favorite.id }
    })
  } catch (e) {
    console.log(err)
  }
}

onMounted(async () => {
  cart.value = JSON.parse(localStorage.getItem('cart') || '[]')

  await fetchItems()
  await fetchFavor()
  items.value = items.value.map((item) => ({
    ...item,
    isAdd: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdd: false
  }))
})
</script>

<template>
  <Banner />
  <div class="p-10">
    <div class="flex justify-between items-center">
      <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

      <div class="flex gap-4">
        <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
          <option value="name">По названию</option>
          <option value="-price">По цене (по убыванию)</option>
          <option value="price">По цене (по возрастанию)</option>
        </select>

        <div class="relative">
          <img class="absolute left-3 top-3" src="/search.svg" alt="search" />
          <input
            @input="onChangeInput"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            type="text"
            placeholder="Поиск"
          />
        </div>
      </div>
    </div>
    <cardList :items="items" @add-to-favor="addToFavor" @add-to-busket="busketChange" />
  </div>
</template>
