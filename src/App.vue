<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="+income" :expenses="+expenses"/>
    <TransactionList @transactionDeleted="handleTransactionDeleted" :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
})

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => {
      return transaction.amount > 0
    })
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => {
      return transaction.amount < 0
    })
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed();
});

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  console.log(generateUniqueId());

  saveToLocalStorage();

  toast.success('Transaction created');
};

const handleTransactionDeleted = (transactionId) => {
  console.log(transactionId)
  transactions.value = transactions.value.filter((transaction) => transaction.id !== transactionId);

  saveToLocalStorage();

  toast.success('Transaction deleted');
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000);
}

// Save to localstorage
const saveToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

</script>