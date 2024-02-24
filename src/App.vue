<script setup>
import { ref } from 'vue'
import ItemCard from './components/ItemCard.vue'

const cart = ref([])
const total = ref(0)
const items = ref([
  { emoji: '🍔', title: 'Бургер', price: 150, isAdding: true, quantity: 0 },
  { emoji: '🍟', title: 'Фри', price: 100, isAdding: true, quantity: 0 },
  { emoji: '🍕', title: 'Пицца', price: 300, isAdding: true, quantity: 0 },
  { emoji: '🌭', title: 'Хотдог', price: 120, isAdding: true, quantity: 0 },
  { emoji: '🍗', title: 'Курица', price: 250, isAdding: true, quantity: 0 },
  { emoji: '🍪', title: 'Печенье', price: 80, isAdding: true, quantity: 0 },
])

const tg = window.Telegram.WebApp
function addToCart(item) {
  if (!tg.MainButton.isVisible) {
    tg.enableClosingConfirmation()
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
    tg.disableClosingConfirmation()
    tg.MainButton.hide()
  }
}
</script>

<template>
  <div class="grid grid-cols-3 m-2">
    <div
      v-for="item in items"
      :key="item.title"
      class="item-container rounded-2xl p-3 m-1"
    >
      <ItemCard
        :emoji="item.emoji"
        :title="item.title"
        :price="item.price"
        :isAdding="item.isAdding"
        :quantity="item.quantity"
        @add-to-cart="addToCart(item)"
        @increase-to-cart="increaseToCart(item)"
        @decrease-from-cart="decreaseFromCart(item)"
      />
    </div>
  </div>
</template>
