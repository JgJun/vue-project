<script setup>
import { computed, ref, reactive } from "vue";

const titleClass = ref("show");
const pile = ref([]);

const deckOfCards = reactive({
  newDeckOfCards: [],
  shuffle() {
    //Build new cards
    if (this.newDeckOfCards.length === 0) {
      let cards = ["Hearts", "Diamonds", "Clubs", "Spades"];
      let values = [
        "Ace",
        "King",
        "Queen",
        "Jack",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "10",
      ];

      let res = [];

      for (let card in cards) {
        for (let value in values) {
          res.push(values[value] + " of " + cards[card]);
        }
      }
      this.newDeckOfCards = res;
    }

    //shuffle cards
    for (let i = this.newDeckOfCards.length - 1; i > 0; i--) {
      let j = Math.floor(Math.random() * (i + 1));
      [this.newDeckOfCards[i], this.newDeckOfCards[j]] = [
        this.newDeckOfCards[j],
        this.newDeckOfCards[i],
      ];
    }
  },
  deal() {
    pile.value.unshift(this.newDeckOfCards.shift());
  },
});

const buttonText = computed(() => {
  if (deckOfCards.newDeckOfCards.length === 0) {
    return "New deck and shuffle";
  }

  return "Shuffle remaining cards";
});

const isDealDisabled = computed(() => deckOfCards.newDeckOfCards.length === 0);
</script>

<template>
  <div>
    <div>
      <button @click="deckOfCards.shuffle()">{{ buttonText }}</button>
      <button :disabled="isDealDisabled" @click="deckOfCards.deal()">
        Deal
      </button>
    </div>
    <p>Remaining Count:{{ deckOfCards.newDeckOfCards.length }}</p>
    <div>
      <div>Cards:</div>
      <div :class="titleClass">
        <ul>
          <li v-for="card in pile" :key="card">
            {{ card }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style>
.show {
  height: 200px;
  width: 200px;
  overflow-y: auto;
}
</style>
