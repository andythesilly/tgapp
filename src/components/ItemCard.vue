<script setup>
const item = defineProps({
  emoji: String,
  title: String,
  price: Number,
  isAdding: Boolean,
  quantity: Number,
})

defineEmits(['add-to-cart', 'increase-to-cart', 'decrease-from-cart'])
</script>

<template>
  <div class="text-7xl">{{ item.emoji }}</div>
  <div class="flex flex-row py-2">
    <div>{{ item.title }}</div>
    <div class="mx-1 hint">·</div>
    <div class="font-bold">{{ item.price }}</div>
  </div>
  <div v-if="item.isAdding" class="flex flex-row justify-center items-center">
    <button class="rounded-full py-1 px-3" @click="$emit('add-to-cart')">
      Добавить
    </button>
  </div>
  <div v-else class="flex flex-row justify-center items-center">
    <button class="rounded-full py-1 px-3" @click="$emit('increase-to-cart')">
      +
    </button>
    <div class="hint px-1">{{ item.quantity }}</div>
    <button class="rounded-full py-1 px-3" @click="$emit('decrease-from-cart')">
      -
    </button>
  </div>
</template>
