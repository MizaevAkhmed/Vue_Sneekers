<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer']);
</script>

<template>
    <header class="flex justify-between border-b border-slate-100 px-10 py-8">
      <router-link to="/">
        <div class="flex items-center gap-4">
          <img src="/public/logo.png" alt="Logo" class="w-10">
          <div>
            <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
            <p class="text-slate-400">Магазин лучших кроссовок</p>
          </div>
        </div>
      </router-link>
      <ul class="flex items-center gap-10">
        <li @click="() => emit('openDrawer')" class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
          <img src="/public/cart.svg" alt="Cart">
          <span>{{ totalPrice }} руб.</span>
        </li>
        
        <router-link to="/favorites">
          <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
            <img src="/public/heart.svg" alt="Cart">
            <span>Закладки</span>
          </li>
        </router-link>
        
        <router-link to="/profile">
          <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
            <img src="/public/profile.svg" alt="Cart">
            <span>Профиль</span>
          </li>
        </router-link>
      </ul>
    </header>
</template>