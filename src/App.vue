<script>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
export default {
  components: {
    Header,
    Balance,
    IncomeExpenses,
  },
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance />
    <IncomeExpenses />
    <TransitionList />
  </div>
</template>
