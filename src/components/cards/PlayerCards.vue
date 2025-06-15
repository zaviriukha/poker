<script setup>
const props = defineProps({
  cards: Array,
  playerName: String,
  visible: Boolean
})

const emit = defineEmits(['update:visible'])

function toggle() {
  emit('update:visible', !props.visible)
}
</script>

<template>
  <div class="text-center text-white">
    <h2 class="font-semibold mb-2">{{ playerName }}</h2>

    <div class="flex gap-2 justify-center mb-2">
      <div
          v-for="(card, index) in cards"
          :key="playerName + '-' + index"
          class="w-12 h-16 flex items-center justify-center text-2xl border rounded bg-white text-black"
      >
        <span v-if="visible">{{ card }}</span>
        <span v-else>🂠</span>
      </div>
    </div>

    <button
        v-if="cards.length"
        @click="toggle"
        class="text-xs px-2 py-1 bg-gray-700 text-white rounded hover:bg-gray-600"
    >
      {{ visible ? 'Hide' : 'Show' }} cards
    </button>
  </div>
</template>
