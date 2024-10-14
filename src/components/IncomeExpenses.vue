 <template>
  <div class="inc-exp-container">
    <div>
      <h4>Доход</h4>
      <p id="money-plus" class="money plus">+${{ income }}</p>
    </div>
    <div>
      <h4>Расход</h4>
      <p id="money-minus" class="money minus">-${{ expenses }}</p>
    </div>
    <IncomeExpenseChart :income="income" :expenses="expenses" />
  </div>
</template>

<script setup>
import { defineProps } from 'vue'
import IncomeExpenseChart from './IncomeExpenseChart.vue'

const props = defineProps({
  income: {
    type: Number,
    required: true,
  },
  expenses: {
    type: Number,
    required: true,
  },
})
</script>

<style scoped>
.inc-exp-container {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.money {
  font-size: 20px;
  margin: 5px 0;
}

.plus {
  color: #2ecc71;
}

.minus {
  color: #e74c3c;
}

.chart-container {
  margin-top: 20px;
}
</style>
