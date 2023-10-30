<template>
  <Line :data="chartData" :options="chartOptions" />
</template>

<script>
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)


export default {
  name: 'LineCharts',
  components: {
    // eslint-disable-next-line vue/no-reserved-component-names
    Line
  },
  props: {
    chartData: {
      type: Object,
      required: true
    },
    chartOptions: {
      responsive: true,
      maintainAspectRatio: false,
      legend: {
        display: false
      },
      animations: {
        tension: {
          duration: 1000,
          easing: 'linear',
          from: 1,
          to: 0,
          loop: true
        }
      }, scales: {
        y: {
          min: 0,
          max: 100
        }
      }
    }
  }
}
</script>