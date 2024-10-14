  <script setup>
  import { Doughnut } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement, CategoryScale, LinearScale } from 'chart.js'
  import { computed } from 'vue'
  
  ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale, LinearScale)
  
  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
    },
  })
  
  const chartData = computed(() => {
    const categoryTotals = {}
    props.transactions.forEach(t => {
      if (t.amount > 0) {
        if (categoryTotals[t.category]) {
          categoryTotals[t.category] += t.amount
        } else {
          categoryTotals[t.category] = t.amount
        }
      }
    })
  
    return {
      labels: Object.keys(categoryTotals),
      datasets: [
        {
          label: 'Доходы по категориям',
          backgroundColor: generateColors(Object.keys(categoryTotals).length),
          data: Object.values(categoryTotals),
        },
      ],
    }
  })
  
  const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
  }
  
  function generateColors(num) {
    const colors = []
    for (let i = 0; i < num; i++) {
      const r = Math.floor(Math.random() * 255)
      const g = Math.floor(Math.random() * 255)
      const b = Math.floor(Math.random() * 255)
      colors.push(`rgba(${r}, ${g}, ${b}, 0.6)`)
    }
    return colors
  }
  </script>
  
  <template>
    <div class="chart-container">
      <Doughnut :chart-data="chartData" :options="chartOptions" />
    </div>
  </template>

  <style scoped>
  .chart-container {
    position: relative;
    height: 400px;
    width: 100%;
    margin-top: 40px;
  }
  </style>