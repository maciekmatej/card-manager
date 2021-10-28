<template>
<div class="bg-dimm">
<div class="page-container">
  <vee-form
    @submit="register"
    :validation-schema="schema"
  >
    <!-- Name -->
    <div class="mb-3">
      <label class="inline-block mb-2">number</label>
      <vee-field
        name="nr"
        type="number"

        placeholder="Enter card number"
      />
      <error-message class="text-red-600" name="nr" />
    </div>
    <!-- Email -->
    <div class="mb-3">
      <label class="inline-block mb-2">PIN</label>
      <vee-field
        type="number"
        name="pin"

        placeholder="Enter PIN"
      />
      <error-message class="text-red-600" name="pin" />
    </div>
    <!-- Age -->
    <div class="mb-3">
      <label class="inline-block mb-2">activation date</label>
      <vee-field
        name="activation_date"
        type="datetime-local"

      />
      <error-message class="text-red-600" name="activation_date" />
    </div>
    <!-- Password -->
    <div class="mb-3">
      <label for="pass" class="inline-block mb-2">valid to date</label>
      <vee-field
        name="validto_date"
        type="datetime-local"
        placeholder="enter expire date"
      />
      <error-message class="text-red-600" name="activation_date" />
    </div>
    <!-- Confirm Password -->
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      schema: {
        nr: 'required|min:2|max:20',
        pin: 'required|min:4|max:4',
        activation_date: 'required',
        validto_date: 'required',
        balance: 'required',
      },
    };
  },
  emits: ['reloadData'],
  methods: {
    async register(data) {
      try {
        await fetch('https://test.boncard.pl/rekrutacja/api/create.php', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(data),
        });
      } catch (e) {
        console.log(e);
        return;
      }
      console.log('succes');
      this.$emit('reloadData');
    },
  },
};
</script>

<style>
.page-container {

    width: fit-content;
    height: fit-content;
    background-color: white;
    box-shadow: 0 0 10px rgb(0, 0, 0);
}
form {
    padding: 100px;
}
.bg-dimm {
display: flex;
justify-content: center;
align-items: center;
position: fixed;
top: 0;
width: 100%;
height: 100vh;
background-color: rgba(0, 0, 0, 0.712);
z-index: 9999;
}
</style>
