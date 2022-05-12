<template>
  <Pie
    v-if="boolRender"
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
</template>

<script>
import { Pie } from 'vue-chartjs'
import { Chart, registerables } from 'chart.js';
Chart.register(...registerables);

export default {
  name: 'pieChart',
  components: { Pie },
  props: {
    percent: {
      type: Array,
      default: [30, 30, 30],
    },
    valuesCoin: {
      type: String,  
    },
    chartId: {
      type: String,
      default: 'bar-chart'
    },
    datasetIdKey: {
      type: String,
      default: 'label'
    },
    width: {
      type: Number,
      default: 200
    },
    height: {
      type: Number,
      default: 200
    },
    cssClasses: {
      default: '',
      type: String
    },
    styles: {
      type: Object,
      default: () => {}
    },
    plugins: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      boolRender: true,
      chartData: {
        labels: [
            'Bitcoin',
            'Ethereum',
            'US Dollar'
        ],
        datasets: [
          {
            label: ``,
            data: [
                this.percent[0],
                this.percent[1],
                this.percent[2],
            ],
            fill: false,
            backgroundColor: [
                '#0d4175',
                '#9f2441',
                '#1d1b28'
              ],
            tension: 0.1,
            hoverOffset: 1,
            borderWidth: 1,
          },
        ],
      },
      chartOptions: {
        responsive: true
      },
    }
  },
}
</script>