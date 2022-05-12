<template>
  <Line
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
import { Line } from 'vue-chartjs'
import { Chart, registerables } from 'chart.js';
Chart.register(...registerables);

export default {
  name: 'lineChart',
  components: { Line },
  props: {
    colorLine: {
      type: String,
    },
    currencyName: {
      type: String,
    },
    currenciesData: {
      type: Array,
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
      default: 380
    },
    height: {
      type: Number,
      default: 250
    },
    cssClasses: {
      default: '',
      type: String
    },
    styles: {
      type: Object,
      default: () => {
      }
    },
    plugins: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      chartData: {
        labels: [
          `${this.currenciesData[0].day}`,
          `${this.currenciesData[1].day}`, 
          `${this.currenciesData[2].day}`, 
          `${this.currenciesData[3].day}`, 
          `${this.currenciesData[4].day}`, 
          `${this.currenciesData[5].day}`, 
          `${this.currenciesData[6].day}`, 
          `${this.currenciesData[7].day}`, 
          `${this.currenciesData[8].day}`, 
          `${this.currenciesData[9].day}`, 
          `${this.currenciesData[10].day}`, 
          `${this.currenciesData[11].day}`, 
          `${this.currenciesData[12].day}`, 
          `${this.currenciesData[13].day}`, 
        ],
        datasets: [
          {
            label: `${this.currencyName}`,
            data: [
              +`${this.currenciesData[0].value}`,
              +`${this.currenciesData[1].value}`, 
              +`${this.currenciesData[2].value}`, 
              +`${this.currenciesData[3].value}`, 
              +`${this.currenciesData[4].value}`, 
              +`${this.currenciesData[5].value}`, 
              +`${this.currenciesData[6].value}`, 
              +`${this.currenciesData[7].value}`, 
              +`${this.currenciesData[8].value}`, 
              +`${this.currenciesData[9].value}`, 
              +`${this.currenciesData[10].value}`, 
              +`${this.currenciesData[11].value}`, 
              +`${this.currenciesData[12].value}`, 
              +`${this.currenciesData[13].value}`, 
            ],
            fill: false,
            borderColor: `${this.colorLine}`,
            tension: 0.1
          },
          
        ],
      },
      chartOptions: {
        responsive: true,
      },
    }
  },
}
</script>