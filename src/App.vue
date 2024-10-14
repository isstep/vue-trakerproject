<script>
import { ref, computed, watch } from "vue";
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import IncomeExpenseChart from "./components/IncomeExpenseChart.vue";
import CategoryChart from "./components/CategoryChart.vue";
import Filter from "./components/Filter.vue";

export default {
  name: "App",
  components: {
    Header,
    Balance,
    IncomeExpenses,
    TransactionList,
    AddTransaction,
    IncomeExpenseChart,
    CategoryChart,
    Filter,
  },
  setup() {
    const transactions = ref(
      JSON.parse(localStorage.getItem("transactions")) || [
        {
          id: 1,
          text: "Зарплата",
          amount: 5000,
          category: "Зарплата",
          date: "2024-04-01",
        },
        {
          id: 2,
          text: "Кафе",
          amount: -150,
          category: "Еда",
          date: "2024-04-02",
        },
        {
          id: 3,
          text: "Бензин",
          amount: -200,
          category: "Транспорт",
          date: "2024-04-03",
        },
      ]
    );

    const categories = ref([
      "Зарплата",
      "Еда",
      "Транспорт",
      "Развлечения",
      "Прочее",
    ]);

    const handleAddTransaction = (transaction) => {
      transactions.value.unshift(transaction);
    };

    const handleDeleteTransaction = (id) => {
      transactions.value = transactions.value.filter((t) => t.id !== id);
    };

    watch(
      transactions,
      (newTransactions) => {
        localStorage.setItem("transactions", JSON.stringify(newTransactions));
        calculateTotals();
      },
      { deep: true }
    );

    const income = ref(0);
    const expenses = ref(0);

    const calculateTotals = () => {
      const amounts = transactions.value.map((t) => t.amount);
      income.value = amounts
        .filter((a) => a > 0)
        .reduce((acc, a) => acc + a, 0);
      expenses.value = Math.abs(
        amounts.filter((a) => a < 0).reduce((acc, a) => acc + a, 0)
      );
    };

    calculateTotals();
    const selectedCategory = ref("Все");
    const searchQuery = ref("");

    const handleFilter = (category) => {
      selectedCategory.value = category;
    };

    const handleSearch = (query) => {
      searchQuery.value = query;
    };

    const filteredTransactions = computed(() => {
      return transactions.value.filter((t) => {
        const matchesCategory =
          selectedCategory.value === "Все" ||
          t.category === selectedCategory.value;
        const matchesSearch = t.text
          .toLowerCase()
          .includes(searchQuery.value.toLowerCase());
        return matchesCategory && matchesSearch;
      });
    });

    return {
      transactions,
      handleAddTransaction,
      handleDeleteTransaction,
      income,
      expenses,
      categories: categories.value,
      filteredTransactions,
      handleFilter,
      handleSearch,
    };
  },
};
</script>

<style>
@import "./assets/styles.css";
.container {
  max-width: 800px;
  margin: 30px auto;
  padding: 0 20px;
  background: #fff;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.list {
  list-style-type: none;
  padding: 0;
  margin-top: 20px;
}

@media (max-width: 600px) {
  .container {
    padding: 10px;
  }

  .inc-exp-container {
    flex-direction: column;
    align-items: flex-start;
  }

  .add-transaction,
  .filter {
    width: 100%;
  }
}
</style>

<template>
  <Header />
  <div class="container">
    <Balance :transactions="transactions" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <AddTransaction
      :categories="categories"
      @addTransaction="handleAddTransaction"
    />
    <Filter
      :categories="categories"
      @filter="handleFilter"
      @search="handleSearch"
    />
    <transition-group name="list" tag="ul" class="list">
      <TransactionList
        v-for="transaction in filteredTransactions"
        :key="transaction.id"
        :transaction="transaction"
        @transactionDeleted="handleDeleteTransaction"
      />
    </transition-group>
    <IncomeExpenseChart :income="income" :expenses="expenses" />
    <CategoryChart :transactions="transactions" />
  </div>
</template>
