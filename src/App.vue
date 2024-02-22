<script setup>
import { ref } from 'vue'

const cart = ref([])
const total = ref(0)
const items = ref([
  { emoji: '🍔', title: 'Бургер', price: 150, isAdding: true, quantity: 0 },
  { emoji: '🍟', title: 'Фри', price: 100, isAdding: true, quantity: 0 },
  { emoji: '🍕', title: 'Пицца', price: 300, isAdding: true, quantity: 0 },
])

const tg = window.Telegram.WebApp
function addToCart(item) {
  if (!tg.MainButton.isVisible) {
    tg.MainButton.show()
  }

  cart.value.push(item)

  item.isAdding = false
  item.quantity = 1

  total.value += item.price
  tg.MainButton.setText(total.value)
}

function increaseToCart(item) {
  item.quantity++

  total.value += item.price
  tg.MainButton.setText(total.value)
}

function decreaseFromCart(item) {
  item.quantity--
  total.value -= item.price
  tg.MainButton.setText(total.value)

  if (item.quantity === 0) {
    const index = cart.value.indexOf(item)
    if (index != -1) {
      cart.value.splice(index, 1)
    }

    item.isAdding = true
  }

  if (total.value == 0) {
    tg.MainButton.hide()
  }
}
</script>

<template>
  <div class="grid grid-cols-3 m-2">
    <div
      v-for="item in items"
      class="item-container flex flex-col items-center justify-center rounded-2xl p-3 m-1"
    >
      <div class="text-7xl">{{ item.emoji }}</div>
      <div class="flex flex-row py-2">
        <div>{{ item.title }}</div>
        <div class="mx-1 hint">·</div>
        <div class="font-bold">{{ item.price }}</div>
      </div>
      <div
        v-if="item.isAdding"
        class="flex flex-row justify-center items-center"
      >
        <button class="rounded-full py-1 px-3" @click="addToCart(item)">
          Добавить
        </button>
      </div>
      <div v-else class="flex flex-row justify-center items-center">
        <button class="rounded-full py-1 px-3" @click="increaseToCart(item)">
          +
        </button>
        <div class="hint px-1">{{ item.quantity }}</div>
        <button class="rounded-full py-1 px-3" @click="decreaseFromCart(item)">
          -
        </button>
      </div>
    </div>
  </div>
</template>
