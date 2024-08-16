<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionsList
      @transactionDeleted="handleTransactionDeleted"
      :transactions="transactions"
    />
    <AddTransactions @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";

import AddTransactions from "./components/AddTransactions.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionsList from "./components/TransactionsList.vue";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, t) => {
    return acc + t.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((t) => t.amount > 0)
    .reduce((acc, t) => {
      return acc + t.amount;
    }, 0);
});

const expense = computed(() => {
  return transactions.value
    .filter((t) => t.amount < 0)
    .reduce((acc, t) => {
      return acc + t.amount;
    }, 0);
});

// Add Transatction
const handleTransactionSubmitted = (transactionToSubmit) => {
  transactions.value.push({
    id: generateId(),
    text: transactionToSubmit.text,
    amount: transactionToSubmit.value,
  });

  saveTransactionsToLocalStorage();
};

// Generate ID
const generateId = () => {
  return Math.floor(Math.random() * 100000);
};

// Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((t) => {
    return t.id !== id;
  });

  toast.success("Transação removida com sucesso!");

  saveTransactionsToLocalStorage();
};

// Save to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
