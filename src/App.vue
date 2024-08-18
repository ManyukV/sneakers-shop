<script setup>
import axios from 'axios'
import { onMounted, ref, watch, provide, computed } from 'vue'
import Header from './components/header.vue'
import modalBusket from './components/modal-busket.vue'

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

function addToBusket(item) {
  cart.value.push(item)
  item.isAdd = true
}

function removeFromBusket(item) {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdd = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

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
    :is-creating-order="isCreatingOrder.value"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <Header :total-price="totalPrice" @open-modal="openModal" />

    <RouterView />
  </div>
</template>

<style scoped></style>
