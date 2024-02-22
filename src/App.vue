<script setup>
import { ref } from 'vue'

const cart = ref([])
const total = ref(0)
const items = ref([
  { emoji: '🍔', title: 'Бургер', price: 150 },
  { emoji: '🍟', title: 'Фри', price: 100 },
  { emoji: '🍕', title: 'Пицца', price: 300 },
])

const tg = window.Telegram.WebApp
function addToCart(item) {
  if (!tg.MainButton.isVisible) {
    tg.MainButton.show()
  }

  cart.value.push(item)
  total.value += item.price

  tg.MainButton.setText(total.value)
}
</script>

<template>
  <div class="grid grid-cols-3 m-5">
    <div
      v-for="item in items"
      class="item-container flex flex-col items-center justify-center rounded-2xl p-3 m-1"
    >
      <div class="text-7xl">{{ item.emoji }}</div>
      <div class="flex flex-row py-2">
        <div>{{ item.title }}</div>
        <div class="mx-1">·</div>
        <div class="font-bold">{{ item.price }}</div>
      </div>
      <div>
        <button class="rounded-full py-1 px-3" @click="addToCart(item)">
          Добавить
        </button>
      </div>
    </div>
  </div>
</template>
