<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'
import ModaBusketTitle from './modal-busket-title.vue'
import ModalBusketList from './modal-busket-list.vue'
import InfoBlock from './info-block.vue'

const props = defineProps({
  totalPrice: Number
})

const { cart, closeModal } = inject('modal')

const isCreatingOrder = ref(false)
const orderId = ref(null)

async function createOrder() {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://aa61e44bd2676303.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })
    cart.value = []
    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)

const buttonDisable = computed(() =>
  props.isCreatingOrder ? true : props.totalPrice ? false : true
)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <ModaBusketTitle />
    <ModalBusketList />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пуста"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ ${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-if="totalPrice" class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} BYN</b>
      </div>
      <div class="flex gap-2">
        <span>Доставка:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>7 BYN</b>
      </div>

      <button
        :disabled="buttonDisable"
        @click="createOrder"
        class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-400 cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
