<template>
  <div id="app">
    <div class="current">
      {{ currentCart }}
    </div>

    <ul class="board clearfix">
      <li v-for="(card, c) in allCards" :key="c" :class="{draw: isCardDraw(card)}">
       {{ renderCard(card) }}
     </li>
    </ul>

    <div>
      Nombre de cartes restantes: {{ numCardsLeft }}
    </div>
    <div>
      <button v-if="numCardsLeft" @click.stop.prevent="next">Suivante</button>
    </div>
  </div>
</template>

<style>
body {
  text-align: center;
}
.current {
  font-size: 16em;
}
.clearfix:after {
   content: " ";
   visibility: hidden;
   display: block;
   height: 0;
   clear: both;
}
.board li:nth-child(15n+1) {
  clear: left;
}
.board li {
  list-style: none;
  float: left;
  margin: 10px;
  width: 4%;
}
.draw {
  color: red;
}
</style>

<script>
import _ from 'lodash';

const numTotalCards = 75;
const letters = 'BINGO';
const allCards = _.times(numTotalCards, v => Number(v) + 1);
const renderCard = card => `${letters[Math.floor((card - 1) / 15)]}${card}`;

export default {
  name: 'app',
  data () {
    const cards = _.shuffle(allCards);
    const index = 0;
    const drawCards = [cards[index]];
    return {
      cards,
      drawCards,
      index,
    };
  },
  computed: {
    currentCart() {
      return renderCard(this.cards[this.index]);
    },
    numCardsLeft() {
      return numTotalCards - this.index - 1;
    },
    allCards() {
      return allCards;
    }
  },
  methods: {
    renderCard,
    next() {
      this.index++;
      this.drawCards.push(this.cards[this.index]);
    },
    isCardDraw(card) {
      return _.includes(this.drawCards, card);
    }
  }
};
</script>
