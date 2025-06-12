<script setup>
defineProps({
  playerName: String,
  cards: Array,
  cardsVisible: Boolean
})
</script>

<template>
  <div class="text-center text-white">
    <h2 class="font-semibold mb-2">{{ playerName }}</h2>
    <div class="flex gap-2">
      <div
          v-for="(card, index) in cards"
          :key="playerName + '-' + index"
          class="w-12 h-16 flex items-center justify-center text-2xl border rounded bg-white text-black"
      >
        <span v-if="cardsVisible">{{ card }}</span>
        <span v-else>🂠</span>
      </div>
    </div>
  </div>
</template>
