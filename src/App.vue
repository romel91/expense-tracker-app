<script setup>
import { ref, onMounted, watch } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions');
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

watch(transactions, (newVal) => {
  localStorage.setItem('transactions', JSON.stringify(newVal));
}, { deep: true });

const addTransaction = (transaction) => {
  transactions.value.push(transaction);
};

const deleteTransaction = (index) => {
  transactions.value.splice(index, 1);
};
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center">Expense Tracker</h2>
    <AddTransaction @add-transaction="addTransaction" />
    <TransactionList :transactions="transactions" @delete-transaction="deleteTransaction" />
  </div>
</template>

<style scoped>

</style>
