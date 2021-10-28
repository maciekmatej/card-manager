<template>
  <div class="home">
    <button @click="toggleAddCard">Add new</button>
    <div class="container">
    <Card v-for="card in cards" :key="card.id" :card="card" @reload-data="fetchData"/>
    </div>
  </div>
  <AddCard v-if="addNewCard" @reload-data="fetchData"/>
</template>

<script>
// @ is an alias to /src
import Card from '@/components/Card.vue';
import AddCard from '@/components/AddCard.vue';

export default {
  name: 'Home',
  data() {
    return {
      cards: [],
      addNewCard: false,
    };
  },
  components: {
    Card,
    AddCard,
  },
  methods: {
    toggleAddCard() {
      this.addNewCard = !this.addNewCard;
    },
    async fetchData() {
      console.log('emited');
      const data = await fetch('https://test.boncard.pl/rekrutacja/api/read.php');
      const res = await data.json();
      this.cards = res.body;
      this.addNewCard = false;
    },
  },
  async mounted() {
    await this.fetchData();
  },

};
</script>
<style scoped>
.container {
  display: flex;
  width: 100%;
  flex-wrap: wrap;
}
</style>
