<script>
import { Bar } from 'vue-chartjs'

export default {
  name: 'Chart',
  extends: Bar,
  props: {
    getLocation: {
      type: String,
      default: '',
    },
    getPeriod: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      location: this.getLocation,
      period: this.getPeriod,
      result: [],
      datetime: [],
      testedPositive: [],
      ern: [],
      sma5: [],
      sma10: [],
    }
  },
  mounted() {
    this.drawGraph()
  },
  methods: {
    convertCsvStringToArray(str) {
      return str.split('\n').map((s) => s.split(','))
    },
    loadCsv() {
      const csv = `https://raw.githubusercontent.com/broccolingual/Covid-19-Data-Analysis/master/output/${
        this.location.charAt(0).toUpperCase() +
        this.location.slice(1).toLowerCase()
      }.csv`
      const txt = new XMLHttpRequest()
      txt.open('get', csv, false)
      txt.send()
      const arr = txt.responseText.split('\n')
      const data = []
      for (let h = 0; h < arr.length; h++) {
        if (arr[h] === '') break
        data[h] = arr[h].split(',')
      }
      data.shift()
      this.result = data.slice(-1 * parseInt(this.period, 10))
    },
    getData() {
      this.getDatetimeLabels()
      this.getTestedPositive()
      this.getERN()
      this.getSMA5()
    },
    getDatetimeLabels() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(this.result[i][0])
      }
      this.datetime = tmp
    },
    getTestedPositive() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(parseInt(this.result[i][8], 10))
      }
      this.testedPositive = tmp
    },
    getERN() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(parseFloat(this.result[i][7]))
      }
      this.ern = tmp
    },
    getSMA5() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(parseInt(this.result[i][9], 10))
      }
      this.sma5 = tmp
    },
    getSMA10() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(parseInt(this.result[i][10], 10))
      }
      this.sma10 = tmp
    },
    drawGraph() {
      this.loadCsv()
      this.getData()

      const data = {
        labels: this.datetime,
        datasets: [
          {
            label: 'Tested Positive',
            data: this.testedPositive,
            borderColor: 'rgba(227, 102, 169, 0.8)',
            backgroundColor: 'rgba(227, 102, 169, 0.4)',
            fill: true,
            type: 'line',
            lineTention: 0.3,
            yAxisID: 'y-axis-1',
          },
          {
            label: 'Effective Reproduction Number',
            data: this.ern,
            borderColor: 'rgba(124, 124, 196, 0.8)',
            fill: false,
            type: 'line',
            lineTention: 0.3,
            yAxisID: 'y-axis-2',
          },
          {
            label: 'Tested Positive(SMA5)',
            data: this.sma5,
            borderColor: '#F1F1E3',
            fill: false,
            type: 'line',
            lineTention: 0.3,
            yAxisID: 'y-axis-1',
          },
        ],
      }
      const options = {
        title: {
          display: true,
          text: `${
            this.location.charAt(0).toUpperCase() +
            this.location.slice(1).toLowerCase()
          } - Last ${this.period} days`,
          position: 'bottom',
          fontColor: 'rgba(241, 241, 227, 0.8)',
        },
        legend: {
          display: true,
          labels: {
            fontColor: 'rgba(241, 241, 227, 0.8)',
          },
        },
        responsive: true,
        scales: {
          xAxes: [
            {
              scaleLabel: {
                display: true,
                labelString: 'Date',
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
              gridLines: {
                color: 'rgba(241, 241, 227, 0.2)',
              },
              ticks: {
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
            },
          ],
          yAxes: [
            {
              id: 'y-axis-1',
              type: 'linear',
              position: 'left',
              scaleLabel: {
                display: true,
                labelString: 'Tested Positive',
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
              gridLines: {
                color: 'rgba(241, 241, 227, 0.2)',
              },
              ticks: {
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
            },
            {
              id: 'y-axis-2',
              type: 'linear',
              position: 'right',
              scaleLabel: {
                display: true,
                labelString: 'Effective Reproduction Number',
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
              gridLines: {
                color: 'rgba(241, 241, 227, 0.2)',
              },
              ticks: {
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
            },
          ],
        },
      }
      this.renderChart(data, options)
    },
  },
}
</script>
