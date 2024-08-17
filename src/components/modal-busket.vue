<script setup>
import modaBusketTitle from './modal-busket-title.vue'
import modalBusketList from './modal-busket-list.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <modaBusketTitle />
    <modalBusketList />

    <div class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>1200 BYN</b>
      </div>
      <div class="flex gap-2">
        <span>Доставка:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>7 BYN</b>
      </div>

      <button
        disabled=""
        class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-400 cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
