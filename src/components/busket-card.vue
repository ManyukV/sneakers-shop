<script setup>
const emit = defineEmits(['removeFromBusket'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})
</script>

<template>
  <div class="flex items-center w-full border border-separate-100 p-4 rounded-xl gap-4 mb-3">
    <img class="w-16 h-16" :src="imageUrl" alt="sneaker" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} BYN</b>
        <img
          @click="emit('removeFromBusket')"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="close"
        />
      </div>
    </div>
  </div>
</template>
