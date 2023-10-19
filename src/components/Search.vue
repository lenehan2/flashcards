<template>
  <v-autocomplete
    label="Autocomplete"
    multiple
    :search="search"
    chips
    closeable-chips
    v-model="value"
    :items="items"
    @change="searchCards(event)"
  ></v-autocomplete>
  <!-- end of search box -->
</template>

<script lang="ts">
import cardsData from '../../data/cards.json'

const tagsMap: Record<string, unknown> = {}

cardsData.forEach((card) => {
  card.tags.forEach((t) => {
    tagsMap[t] = 1
  })
})
const tags = Object.keys(tagsMap)

export default {
  name: 'Card-Search',
  data() {
    return {
      search: this.search || '',
      value: this.value || [],
      items: tags
    }
  },
  watch: {
    value: () => {
      console.log('Here!')
    }
  },
  methods: {
    searchCards() {
      console.log('Here!')
      this.$emit('searchTrigger', this.search)
    }
  }
}
</script>

<style scoped lang="scss"></style>
