<script setup>
import { ref, onMounted } from 'vue'
import PlayerCards from "@/components/cards/PlayerCards.vue";

const deck = ref([])
const fullDeck = [
  '🂡','🂢','🂣','🂤','🂥','🂦','🂧','🂨','🂩','🂪','🂫','🂭','🂮',
  '🂱','🂲','🂳','🂴','🂵','🂶','🂷','🂸','🂹','🂺','🂻','🂽','🂾',
  '🃁','🃂','🃃','🃄','🃅','🃆','🃇','🃈','🃉','🃊','🃋','🃍','🃎',
  '🃑','🃒','🃓','🃔','🃕','🃖','🃗','🃘','🃙','🃚','🃛','🃝','🃞'
]
const deckValues = {
  '🂡': 14,'🂢': 2,'🂣': 3,'🂤': 4,'🂥': 5,'🂦': 6,'🂧': 7,'🂨': 8,'🂩': 9,'🂪': 10,'🂫': 11,'🂭': 12,'🂮': 13,
  '🂱': 14,'🂲': 2,'🂳': 3,'🂴': 4,'🂵': 5,'🂶': 6,'🂷': 7,'🂸': 8,'🂹': 9,'🂺': 10,'🂻': 11,'🂽': 12,'🂾': 13,
  '🃁': 14,'🃂': 2,'🃃': 3,'🃄': 4,'🃅': 5,'🃆': 6,'🃇': 7,'🃈': 8,'🃉': 9,'🃊': 10,'🃋': 11,'🃍': 12,'🃎': 13,
  '🃑': 14,'🃒': 2,'🃓': 3,'🃔': 4,'🃕': 5,'🃖': 6,'🃗': 7,'🃘': 8,'🃙': 9,'🃚': 10,'🃛': 11,'🃝': 12,'🃞': 13
}
// Для прототипуваня використовується 2 масиви (спадок від першого варіанту)
// fullDeck масив з іконками для сортування через Math.random() та для .splice() у роздачі
// deckValues - масив у якому кожна картка має значення. Потрібно для діставання значення карт через cardValues[card]
// та для визначення суми у нашому випадку.
// В ідеалі ці 2 масив та об'єкт потрібно звести в один масив об'єктів які пересилають нам всю інформацію про колоду.
// { symbol: '🂡', value: 14 }

const player1 = ref([]) // базово пустий масив для карт гравця
const player2 = ref([]) // базово пустий масив для карт гравця
const cardsVisible = ref(true) // статус відкритості карт гравців на столі
const result = ref('') // результат переможця

// тасуемо fullDeck а також з цього починається гра - стіл пустий, колода тасована
function resetDeck() {
  deck.value = [...fullDeck].sort(() => Math.random() - 0.5) // тасуємо колоду
  player1.value = [] // порожня колода гравця
  player2.value = [] // порожня колода гравця
  result.value = '' // значення переможця пусте
}

// визначення переможця через порівняння суми карт на руках
function getWinner() {
  const sum = cards => cards.reduce((acc, card) => acc + (deckValues[card] || 0), 0)
  const p1 = sum(player1.value)
  const p2 = sum(player2.value)

  if (p1 > p2) result.value = `Player 1 wins (${p1} vs ${p2}) 🏆`
  else if (p2 > p1) result.value = `Player 2 wins (${p2} vs ${p1}) 🏆`
  else result.value = `It's a draw (${p1} vs ${p2}) 🤝`
}

// здаємо карти
function dealCards() {
  if (deck.value.length < 4) {
    alert("У колоді не залишилось достатньо карт!")
    return
  }
  player1.value = deck.value.splice(0, 2) // здаємо по 2 карти з колоди гравцю, з колоди вони видаляються
  player2.value = deck.value.splice(0, 2) // здаємо по 2 карти з колоди гравцю, з колоди вони видаляються
  getWinner()
}

// перемикання видимості карт гравців
function toggleCards() {
  cardsVisible.value = !cardsVisible.value
}

// роздача карт при завантаженні компонента
onMounted(() => {
  resetDeck()
  // dealCards()
})
</script>


<template>
  <div class="min-h-screen flex items-center justify-center">
    <div class="p-6 max-w-4xl w-full">
      <h1 class="text-2xl font-bold text-center mb-16">Poker Table</h1>
      <div class="h-36 flex justify-between items-center mb-6 border border-white p-12 rounded-full bg-green-700 shadow-lg">
        <PlayerCards :cards="player1" :player-name="'Player 1'" :cards-visible="cardsVisible" />
        <PlayerCards :cards="player2" :player-name="'Player 2'" :cards-visible="cardsVisible" />
      </div>
      <div class="flex justify-center gap-4">
        <div v-if="result" class="text-center mt-6 text-xl font-semibold">
          {{ result }}
        </div>
      </div>
      <div class="flex justify-center gap-4">
        <button @click="toggleCards" class="w-32 px-4 py-2 bg-indigo-600 text-white rounded hover:bg-indigo-700 min-w-20">
          {{ cardsVisible ? 'Hide' : 'Show' }} Cards
        </button>
        <button @click="dealCards" class="w-32 px-4 py-2 bg-green-600 text-white rounded hover:bg-green-700">
          Deal Cards
        </button>
        <button @click="resetDeck" class="w-32 px-4 py-2 bg-red-600 text-white rounded hover:bg-red-700">
          Shuffle New Deck
        </button>
      </div>
    </div>
  </div>
</template>

