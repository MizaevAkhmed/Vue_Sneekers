<script setup>
import { inject } from 'vue'
import CardItem from '@/components/CardItem.vue'

const { cart, removeFromCart } = inject('cart')

</script>

<template>
    <div class="flex flex-col gap-4 justify-between" v-auto-animate>
        <CardItem 
        v-for="item in cart" 
        :key="item.id" 
        :title="item.title"
        :price="item.price"
        :imageUrl="item.imageUrl"
        @onClickRemove="() => removeFromCart(item)"/>
    </div>
</template>