<template>
    <div class="chart-container">
      <Bar :chart-data="chartData" :options="chartOptions" />
    </div>
  </template>
  
  <script setup>
  import { Bar } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
  import { computed } from 'vue'
  
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
  
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
  
  const chartData = computed(() => ({
    labels: ['Доходы', 'Расходы'],
    datasets: [
      {
        label: 'Сумма',
        backgroundColor: ['#2ecc71', '#e74c3c'],
        data: [props.income, props.expenses],
      },
    ],
  }))
  
  const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
  }
  </script>
  
  <style scoped>
  .chart-container {
    position: relative;
    height: 300px;
    width: 100%;
  }
  </style>
  