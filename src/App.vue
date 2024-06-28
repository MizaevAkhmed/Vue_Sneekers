<script setup>
import {ref, provide, watch, computed } from 'vue'
import axios from 'axios'

import Header from '@/components/Header.vue';
import Drawer from '@/components/Drawer.vue';

// Корзина
const cart = ref([])

const drawerOpen = ref(false)

const totalPrice = computed(() => {
  if (Array.isArray(cart.value)) {
    return cart.value.reduce((acc, item) => acc + item.price, 0);
  } else {
    // Handle the case where cart.value is not an array
    console.log('cart.value is not an array:', cart.value);
    return 0;
  }
});
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartButtonDisabled = computed(() =>
  isCreatingOrder.value ? true : totalPrice.value ? false : true
)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
  // console.log(cart)
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(cart, () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})

// Корзина
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :totalPrice="totalPrice" @openDrawer="openDrawer" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
