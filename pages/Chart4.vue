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
      hospitalized: [],
    }
  },
  mounted() {
    this.drawGraph()
  },
  methods: {
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
      this.getHospitalized()
    },
    getDatetimeLabels() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(this.result[i][0])
      }
      this.datetime = tmp
    },
    getHospitalized() {
      const tmp = []
      for (let i = 0; i < this.result.length; i++) {
        tmp.push(parseInt(this.result[i][3], 10))
      }
      this.hospitalized = tmp
    },
    drawGraph() {
      this.loadCsv()
      this.getData()

      const data = {
        labels: this.datetime,
        datasets: [
          {
            label: 'Hospitalized',
            data: this.hospitalized,
            borderColor: 'rgba(227, 102, 169, 0.8)',
            backgroundColor: 'rgba(227, 102, 169, 0.6)',
            fill: true,
            type: 'bar',
            lineTention: 0.3,
            yAxisID: 'y-axis-1',
          },
        ],
      }
      const options = {
        title: {
          display: true,
          position: 'bottom',
          fontColor: 'rgba(241, 241, 227, 0.8)',
        },
        legend: {
          display: true,
          labels: {
            fontColor: 'rgba(241, 241, 227, 0.8)',
          },
        },
        elements: {
          point: {
            radius: 0,
          },
        },
        responsive: true,
        scales: {
          xAxes: [
            {
              scaleLabel: {
                display: true,
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
              gridLines: {
                color: 'rgba(241, 241, 227, 0.2)',
                display: false,
              },
              ticks: {
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
              stacked: false,
            },
          ],
          yAxes: [
            {
              id: 'y-axis-1',
              type: 'linear',
              position: 'left',
              scaleLabel: {
                display: true,
                labelString: 'Hospitalized',
                fontColor: 'rgba(241, 241, 227, 0.8)',
              },
              gridLines: {
                color: 'rgba(241, 241, 227, 0.2)',
                drawBorder: false,
                borderDash: [5, 5],
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
