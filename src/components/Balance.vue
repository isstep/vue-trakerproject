<template>
  <div class="balance">
    <h4>Баланс</h4>
    <h1>${{ balance }}</h1>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const balance = computed(() => {
  return props.transactions.reduce((acc, t) => acc + t.amount, 0);
});
</script>

<style scoped>
.balance {
  background: #fff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  text-align: center;
  animation: fadeIn 1s ease-out;
}

.balance h1 {
  margin: 10px 0 0 0;
  font-size: 2em;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 600px) {
  .balance h1 {
    font-size: 1.5em;
  }
}
</style>
