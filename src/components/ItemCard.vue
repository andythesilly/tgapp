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
  <div
    v-show="!item.isAdding"
    class="counter right-1 top-1 rounded-full px-2 font-bold"
  >
    {{ quantity }}
  </div>
  <div class="text-7xl">{{ item.emoji }}</div>
  <div class="flex flex-row items-center py-2">
    <div class="text-base">{{ item.title }}</div>
    <div class="mx-1 hint">·</div>
    <div class="font-bold text-sm">{{ item.price }}</div>
  </div>
  <div
    v-if="item.isAdding"
    class="flex flex-row justify-center items-center w-[60%]"
  >
    <button
      class="rounded-full py-2 px-3 font-bold"
      @click="$emit('add-to-cart')"
    >
      Добавить
    </button>
  </div>
  <div v-else class="flex flex-row justify-center items-center w-full">
    <button
      class="rounded-full p-2 w-[50%] font-bold mr-1"
      @click="$emit('increase-to-cart')"
    >
      +
    </button>
    <button
      class="rounded-full p-2 w-[50%] font-bold ml-1"
      @click="$emit('decrease-from-cart')"
    >
      -
    </button>
  </div>
</template>
