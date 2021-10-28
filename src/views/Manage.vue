<template>
  {{ card.id }}
  {{ card.nr }}
  {{ card.pin }}
  {{ card.balance }}
  <button @click="getData" type="submit">LOAD</button>
</template>

<script>
export default {

  data() {
    return {
      card: {},
    };
  },
  methods: {
    async getData() {
      const ID = this.$route.params.id;
      console.log(ID);
      try {
        const data = await fetch(`https://test.boncard.pl/rekrutacja/api/single_read.php/?id=${ID}`);
        const res = await data.json();
        this.card = res;
      } catch (e) {
        console.log(e);
      }
      console.log('loaded');
    },
  },
  async created() {
    await this.getData();
  },
};
</script>

<style>

</style>
