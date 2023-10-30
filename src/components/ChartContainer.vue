<template>
  <h1> Chart View</h1>
  <div class="chart-container">
    <div class="bar-chart">
      <BarChart :chartData="chartData" />
    </div>
    <div class="line-chart">
      <LineChart :chartData="chartData" />
    </div>
  </div>
</template>

<script>
import BarChart from './BarChart.vue'
import LineChart from './LineChart.vue'

export default {
  name: 'ChartContainer',
  components: {
    BarChart, LineChart
  },
  props: {
    chartData: {
      type: Object,
      required: true,
    }
  },

}
</script>