<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-bind:class="transaction.amount < 0 ? 'minus' : 'plus'" v-for="transaction in transactions" v-bind:key="transaction.id">
      {{ transaction.text }} <span>$ {{ transaction.amount }}</span>
      <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
    }
  });

  const emit = defineEmits(['transactionDeleted']);

  const deleteTransaction = (transactionId) => {
    emit('transactionDeleted', transactionId)
  }
</script>