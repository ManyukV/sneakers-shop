<script setup>
const props = defineProps({
  id: Number,
  imgUrl: String,
  title: String,
  price: Number,
  isFavor: Boolean,
  isAdd: Boolean,
  onClickAdd: Function,
  onClickFavor: Function
})

const showButton = Boolean(props.onClickAdd)
</script>

<template>
  <div
    class="relative border bg-white border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      v-if="onClickFavor"
      @click="onClickFavor"
      class="absolute top-8 left-8"
      :src="!isFavor ? '/like-1.svg' : '/like-2.svg'"
      alt="like"
    />
    <img class="w-80" :src="imgUrl" alt="sneaker" />
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} BYN</b>
      </div>
      <img
        v-if="onClickFavor"
        @click="onClickAdd"
        :src="!isAdd ? '/plus.svg' : '/checked.svg'"
        alt="plus"
      />
    </div>
  </div>
</template>
