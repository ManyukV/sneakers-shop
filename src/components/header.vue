<script setup>
const emit = defineEmits(['openModal'])
defineProps({
  totalPrice: Number
})
</script>

<template>
  <header class="flex justify-between border-b px-10 py-8">
    <RouterLink to="/"
      ><div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Sneakers</h2>
          <p class="text-slate-500">Магазин крососвок</p>
        </div>
      </div></RouterLink
    >

    <ul class="flex justify-between gap-10">
      <li
        @click="() => emit('openModal')"
        class="flex items-center gap-3 text-gray-500 cursor-pointer hover:text-black"
      >
        <img src="/cart.svg" alt="cart" />
        <span>{{ totalPrice }} BYN</span>
      </li>

      <li class="flex items-center gap-3 text-gray-500 cursor-pointer hover:text-black">
        <RouterLink class="flex justify-between gap-3" to="/favorites">
          <img src="/heart.svg" alt="cart" />
          <span>Избранное</span>
        </RouterLink>
      </li>
    </ul>
  </header>
</template>

<style scoped></style>
