<script setup>
import axios from 'axios'
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import Header from './components/header.vue'
import cardList from './components/card-list.vue'
import Banner from './components/banner.vue'
import modalBusket from './components/modal-busket.vue'

const items = ref([])
const cart = ref([])
const isCreatingOrder = ref(false)

const modalStatus = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

function closeModal() {
  modalStatus.value = false
}

function openModal() {
  modalStatus.value = true
}

const filters = reactive({
  sortBy: 'title',
  search: ''
})

async function fetchFavor() {
  try {
    const { data: favorites } = await axios.get(`https://aa61e44bd2676303.mokky.dev/favorites`)
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.productId === item.id)
      if (!favorite) {
        return item
      }

      return { ...item, isFavor: true, favoriteId: favorite.id }
    })
  } catch (e) {
    console.log(err)
  }
}

async function addToFavor(item) {
  try {
    if (!item.isFavor) {
      const obj = {
        productId: item.id
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

function addToBusket(item) {
  cart.value.push(item)
  item.isAdd = true
}

function removeFromBusket(item) {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdd = false
}

async function createOrder() {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://aa61e44bd2676303.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value
    })
    cart.value = []
    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

function busketChange(item) {
  if (!item.isAdd) {
    addToBusket(item)
  } else {
    removeFromBusket(item)
  }
}

function onChangeSelect(event) {
  filters.sortBy = event.target.value
}

function onChangeInput(event) {
  filters.search = event.target.value
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
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavor()
})

watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdd: false
  }))
})

provide('modal', {
  cart,
  closeModal,
  openModal,
  addToBusket,
  removeFromBusket
})
</script>

<template>
  <modalBusket
    :total-price="totalPrice"
    v-if="modalStatus"
    @create-order="createOrder"
    :is-creating-order="isCreatingOrder.value"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-modal="openModal" />
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
  </div>
</template>

<style scoped></style>
