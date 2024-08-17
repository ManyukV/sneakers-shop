<script setup>
import Header from './components/header.vue'
import cardList from './components/card-list.vue'
import Banner from './components/banner.vue'
import aboutUs from './components/about-us.vue'
</script>

<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />
    <Banner />
    <div class="p-10">
      <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
      <cardList />
    </div>
  </div>
</template>

<style scoped></style>
