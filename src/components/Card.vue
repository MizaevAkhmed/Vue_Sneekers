<script setup>
const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})

const visibleFavoriteButton = Boolean(props.onClickAdd)
</script>

<template>
  <div
    class="relative m-10 border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      v-if="visibleFavoriteButton"
      :src="!isFavorite ? './public/like-1.svg' : './public/like-2.svg'"
      alt="Like 1"
      class="absolute top-8 left-8"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="Sneaker" />
    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>
      <img
        v-if="visibleFavoriteButton"
        :src="!isAdded ? './public/plus.svg' : './public/checked.svg'"
        alt="Plus"
        @click="onClickAdd"
      />
    </div>
  </div>
</template>
