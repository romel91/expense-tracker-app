<template>
    <div>
      <div class="mb-3">
        <label class="form-label">Filter Transactions</label>
        <select class="form-select" v-model="filter">
          <option value="all">All</option>
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>Title</th>
            <th>Amount</th>
            <th>Type</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(transaction, index) in filteredTransactions" :key="index">
            <td>{{ transaction.title }}</td>
            <td :class="amountClass(transaction.amount, transaction.type)">
              ${{ transaction.amount }}
            </td>
            <td :class="typeClass(transaction.type)">{{ transaction.type.toUpperCase() }}</td>
            <td>
              <button class="btn btn-danger" @click="$emit('delete-transaction', index)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
      <p v-if="filteredTransactions.length === 0">No transactions recorded yet.</p>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
 
  const props = defineProps(['transactions']);
  
  const filter = ref('all');
  
  const filteredTransactions = computed(() => {
    if (filter.value === 'income') {
      return props.transactions.filter(t => t.type === 'Income');
    } else if (filter.value === 'expense') {
      return props.transactions.filter(t => t.type === 'Expense');
    }
    return props.transactions;
  });
  
  const amountClass = (amount, type) => {
    let classes = type === 'Income' ? 'text-success' : 'text-danger';
    if (amount >= 500) classes += ' fw-bold';
    return classes;
  };
  
  const typeClass = (type) => type === 'Income' ? 'text-success' : 'text-danger';
  
  </script>
  