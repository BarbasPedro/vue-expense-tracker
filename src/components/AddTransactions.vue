<template>
  <h3>Add new transaction</h3>

  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        v-model="amout"
        placeholder="Enter amount..."
      />
    </div>

    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amout = ref("");

const toast = useToast();

const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value || !amout.value) {
    toast.error("Ambos os campos devem ser preenchidos");
  } else {
    toast.success("Transação registrada com sucesso!");

    const transactionToSubmit = {
      text: text.value,
      value: amout.value,
    };

    emit("transactionSubmitted", transactionToSubmit);
  }

  text.value = "";
  amout.value = "";
};
</script>
