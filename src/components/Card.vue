<template>
    <router-link :to="{ name: 'Manage', params: { id: card.id } }">
        <div class="square">
            <p>{{ card.nr }}</p>
            <p>{{ card.pin }}</p>
            <p>{{ card.validto_date }}</p>
            <p>{{ card.activation_date }}</p>
            <p>{{ card.balance }}</p>
            <button @click="deleteCard">Delete</button>
            <button @click="toggleEdit">Edit</button>
        </div>
    </router-link>

  <div class="container" v-if="editShow">
  <vee-form
    @submit="saveEditCard"
    :validation-schema="schema"
    :initial-values="card"
  >
    <!-- card number -->
    <div class="mb-3">
      <label class="inline-block mb-2">number</label>
      <vee-field
        name="nr"
        type="number"

        placeholder="Enter card number"
      />
      <error-message class="text-red-600" name="nr" />
    </div>
    <!-- PIN -->
    <div class="mb-3">
      <label class="inline-block mb-2">PIN</label>
      <vee-field
        type="number"
        name="pin"
        placeholder="Enter PIN"
      />
      <error-message class="text-red-600" name="pin" />
    </div>
    <!-- activation date -->
    <div class="mb-3">
      <label class="inline-block mb-2">activation date</label>
      <vee-field
        name="activation_date"
        type="datetime-local"
        :value="formatActivationDate(this.card.activation_date)"
      />
      <error-message class="text-red-600" name="activation_date" />
    </div>
    <!-- expire date -->
    <div class="mb-3">
      <label for="pass" class="inline-block mb-2">valid to date</label>
      <vee-field
        name="validto_date"
        type="datetime-local"
        :value="formatActivationDate(this.card.validto_date)"
      />
      <error-message class="text-red-600" name="activation_date" />
    </div>
    <!-- balance -->
    <div class="mb-3">
      <label for="confirm_password" class="inline-block mb-2">balance</label>
      <vee-field
        id="confirm_password"
        name="balance"
        type="number"
        placeholder="balance"
      />
      <error-message class="text-red-600" name="balance" />
    </div>
    <button type="submit">submit</button>
  </vee-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      editShow: false,
      schema: {
        nr: 'required|min:2|max:20',
        pin: 'required|min:4|max:4',
        activation_date: 'required',
        validto_date: 'required',
        balance: 'required',
      },
    };
  },
  name: 'Card',
  props: ['card'],
  emits: ['reloadData'],
  methods: {
    async deleteCard() {
      console.log(JSON.stringify(this.card));
      // eslint-disable-next-line no-restricted-globals, no-alert
      const prompt = confirm('Please confirm that you want to delete this Card');
      if (prompt) {
        try {
          await fetch('http://test.boncard.pl/rekrutacja/api/delete.php', {
            method: 'POST',
            mode: 'cors',
            headers: {
              'Content-Type': 'application/json',
            },
            body: `{"id":${this.card.id}}`,
          });
        } catch (e) {
          console.log(e);
        }
        this.$emit('reloadData', this.name);
      }
    },
    async saveEditCard(data) {
      const editedData = {
        ...data,
        id: this.card.id,
      };
      try {
        await fetch('http://test.boncard.pl/rekrutacja/api/update.php', {
          method: 'POST',
          mode: 'cors',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(editedData),
        });
      } catch (e) {
        console.log(e);
      }
      this.$emit('reloadData');
    },
    toggleEdit() {
      this.editShow = !this.editShow;
    },
    formatActivationDate(element) {
      const date = element;
      const newDate = date.split(' ');
      const formated = `${newDate[0]}T${newDate[1]}`;
      return formated;
    },
  },
};
</script>

<style>
.square {
    width: 340px;
    height: 200px;
    margin: 20px;
    background-color: rgb(182, 172, 131);
    border-radius: 20px;
    box-shadow: 0 0 15px rgba(65, 54, 54, 0.521);
}
a {
    text-decoration: none;
    color: black;

}
</style>
