<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  transaction: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['transactionDeleted'])

const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}
</script>

<template>
  <li
    :class="transaction.amount < 0 ? 'minus' : 'plus'"
  >
    {{ transaction.text }} <span>${{ transaction.amount }}</span>
    <button class="delete-btn" @click="deleteTransaction(transaction.id)">
      x
    </button>
  </li>
</template>

<style scoped>
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #fff;
  padding: 10px;
  margin: 5px 0;
  border-right: 5px solid;
  transition: background 0.3s ease;
}

.plus {
  border-color: #2ecc71;
}

.minus {
  border-color: #e74c3c;
}

.delete-btn {
  background: #e74c3c;
  color: #fff;
  border: none;
  padding: 5px 9px;
  border-radius: 50%;
  cursor: pointer;
  transition: background 0.3s ease;
}

.delete-btn:hover {
  background: #c0392b;
}
</style>
