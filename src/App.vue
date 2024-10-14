<!-- src/App.vue -->
<script>
import { ref } from 'vue'
import Header from "./components/Header.vue"
import Balance from "./components/Balance.vue"
import IncomeExpenses from "./components/IncomeExpenses.vue"
import TransactionList from "./components/TransactionList.vue"

export default {
  components: {
    Header,
    Balance,
    IncomeExpenses,
    TransactionList,
  },
  setup() {
    const transactions = ref([
      { id: 1, text: 'Зарплата', amount: 5000 },
      { id: 2, text: 'Кафе', amount: -150 },
    ])

    const handleDeleteTransaction = (id) => {
      transactions.value = transactions.value.filter(t => t.id !== id)
    }

    const income = ref(0)
    const expenses = ref(0)

    const calculateTotals = () => {
      const amounts = transactions.value.map(t => t.amount)
      income.value = amounts.filter(a => a > 0).reduce((acc, a) => acc + a, 0)
      expenses.value = Math.abs(amounts.filter(a => a < 0).reduce((acc, a) => acc + a, 0))
    }

    calculateTotals()
    return {
      transactions,
      handleDeleteTransaction,
      income,
      expenses,
    }
  }
}
</script>

<template>
  <Header/>
  <div class="container">
    <Balance />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <transition-group name="list" tag="ul" class="list">
      <TransactionList 
        v-for="transaction in transactions" 
        :key="transaction.id"
        :transaction="transaction" 
        @transactionDeleted="handleDeleteTransaction"
      />
    </transition-group>
  </div>
</template>

<style>
@import './assets/styles.css';

.container {
  max-width: 600px;
  margin: 30px auto;
  padding: 0 20px;
}

.list {
  list-style-type: none;
  padding: 0;
}
</style>
