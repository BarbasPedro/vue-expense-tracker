<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="t in transactions"
      :key="t.id"
      :class="t.amount < 0 ? 'minus' : 'plus'"
    >
      {{ t.text }} <span>R$ {{ t.amount }}</span
      ><button class="delete-btn" @click="deleteTransaction(t.id)">x</button>
    </li>
  </ul>
</template>

<script setup>

const emit = defineEmits(['transactionDeleted'])

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}
</script>
