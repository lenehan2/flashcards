<script lang="ts">
import Card from './Card.vue'
import type { ICard } from './Card.vue'
import Search from './Search.vue';
import cardsData from '../../data/cards.json';
const colors = ['-orange', '-red', '-purple', '-blue', '-green']

type ICardRaw = Omit<ICard, 'tags'> & {
    tags: string[];
}
const cards: ICard[] = cardsData.map((card: ICardRaw) => {
  const tags = {};
  card.tags.forEach((t) => {
      tags[t] = true;
  })
  return {
    ...card,
    tags,
    color: `${colors[Math.floor(Math.random() * colors.length)]}`
  }
});

export default {
  name: 'app',
  data() {
    return {
      cards: cards,
      searchWord: '',
    }
  },
  methods: {
    searchCards(keyword: string) {
      this.searchWord = keyword
    },
    deleteCard(id: string) {
      this.cards = this.cards.filter((card) => {
        if (card.id === id && card.liked) {
          this.likes--
        }
        return card.id !== id
      })
    },
    addCard(data: ICard) {
      this.cards.unshift(data)
    }
  },
  computed: {
    filteredData() {
      return this.cards.filter((card) => {
        return card.front.toLowerCase().includes(this.searchWord.trim().toLowerCase())
      })
    }
  },
  components: {
    Card,
    Search
  }
}
</script>

<template>
  <div id="app">
      <Search /> 
    <div class="cards-box">
      <Card
        v-for="card in filteredData"
        :key="card.id"
        :card="card"
        @likeTrigger="recalculateLikes"
        @deleteTrigger="deleteCard"
      ></Card>
    </div>
    <!-- end of cards box -->
  </div>
</template>

<style lang="scss">
@import 'src/assets/sass/_variables.sass';
.page-container {
  margin-left: auto;
  margin-right: auto;
  max-width: 1120px;
  padding-top: 1.5rem;
  padding-left: 1rem;
  padding-right: 1rem;
  overflow: hidden;
}

.top-message {
  font-size: 0.8571rem;
  text-align: left;
  color: $muted;
  .text {
    display: flex;
    align-items: center;
  }
  .icon {
    width: 20px;
    fill: #ff4242;
    margin-right: 9px;
  }
}

.page-header {
  padding-bottom: 40px;
  border-bottom: 1px dashed #dedede;
  .title {
    font-size: 2.286rem;
    font-weight: $bold;
    text-align: center;
    margin-top: 3.5rem;
    margin-bottom: 3rem;
  }
}

.likecounter {
  font-size: 1.143rem;
  font-weight: $bold;
}

.cards-box {
  margin-top: 1rem;
  margin-left: -1.5rem;
  margin-right: -1.5rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
