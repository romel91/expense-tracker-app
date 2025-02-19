<template>
    <form @submit.prevent="handleSubmit">
      <div class="mb-3">
        <label class="form-label">Title</label>
        <input type="text" class="form-control" v-model="title" required />
      </div>
      <div class="mb-3">
        <label class="form-label">Amount</label>
        <input type="number" class="form-control" v-model.number="amount" required min="1" />
      </div>
      <div class="mb-3">
        <label class="form-label">Type</label>
        <select class="form-select" v-model="type" required>
          <option value="Income">Income</option>
          <option value="Expense">Expense</option>
        </select>
      </div>
      <button type="submit" class="btn btn-primary">Add</button>
    </form>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    setup(_, { emit }) {
      const title = ref('');
      const amount = ref(0);
      const type = ref('Income');
  
      const handleSubmit = () => {
        if (amount.value <= 0) {
          alert('Amount must be greater than 0');
          return;
        }
        emit('add-transaction', { title: title.value, amount: amount.value, type: type.value });
        title.value = '';
        amount.value = 0;
        type.value = 'Income';
      };
  
      return { title, amount, type, handleSubmit };
    }
  };
  </script>