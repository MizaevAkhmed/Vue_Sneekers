<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'

import DrawerHead from '../components/DrawerHead.vue'
import CardItemList from '../components/CardItemList.vue'
import infoBlock from '../components/InfoBlock.vue'

const props = defineProps({
    totalPrice:Number,
    vatPrice: Number
})

const { cart, closeDrawer } = inject('cart')

const isCreating = ref([false])
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    
    const { data } = await axios.post(`https://86083240e072363f.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })
    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead/>

        <div v-if="!totalPrice || orderId" class="h-full items-center">
            <info-block
                v-if="orderId" 
                title="Заказ оформлен" 
                :description="`Ваш заказ №${orderId} скоро будет передан курьерской доставке`" 
                image-url="/public/order-success-icon.png"
            />
            <info-block
                v-if="!totalPrice && !orderId"
                title="Корзина пустая" 
                description="Добавьте товары в корзину" 
                image-url="/public/package-icon.png"
            />
        </div>
        <div v-else>
            <CardItemList/>
            <div class="flex flex-col gap-4 my-7">
                <div class="flex gap-2">
                    <span>Итого:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ totalPrice }} руб</b>
                </div>
                <div class="flex gap-2">
                    <span>Налог 5%:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ vatPrice }} руб</b>
                </div>
                <button 
                :disabled="buttonDisabled ? false : true"
                @click="createOrder"
                class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer">Оформить заказ</button>
            </div>
        </div>
    </div>
</template>