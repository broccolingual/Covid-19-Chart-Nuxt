<template>
  <div class="main">
    <h1>Covid 19 Analysis</h1>
    <div class="input-field">
      <form @submit.prevent>
        <div class="location">
          <p>Prefecture</p>
          <input v-model="location" type="text" />
        </div>
        <div class="period">
          <p>Last</p>
          <input v-model="period" type="text" />
          <p>Days</p>
        </div>
        <button type="submit" @click="click()">Search</button>
      </form>
    </div>
    <div class="graph-container">
      <div class="graph">
        <h3>Tested Positive</h3>
        <Chart v-if="show" :get-location="location" :get-period="period" />
      </div>
      <div class="graph">
        <h3>Death(Total)</h3>
        <Chart2 v-if="show" :get-location="location" :get-period="period" />
      </div>
    </div>
    <div class="graph-container">
      <div class="graph">
        <h3>People Tested(Total)</h3>
        <Chart3 v-if="show" :get-location="location" :get-period="period" />
      </div>
      <div class="graph">
        <h3>Hospitalized</h3>
        <Chart4 v-if="show" :get-location="location" :get-period="period" />
      </div>
    </div>
    <p>Author: broccolingual</p>
  </div>
</template>

<script>
import Chart from './Chart'
import Chart2 from './Chart2'
import Chart3 from './Chart3'
import Chart4 from './Chart4'

export default {
  components: {
    Chart,
    Chart2,
    Chart3,
    Chart4,
  },
  data() {
    return {
      show: true,
      location: 'Tokyo',
      period: '90',
    }
  },
  methods: {
    click() {
      console.log(
        'Location: ',
        this.location,
        '\nPeriod: Last',
        this.period,
        'days'
      )
      this.reloadGraph()
    },
    async reloadGraph() {
      this.show = false
      await this.$nextTick()
      this.show = true
    },
  },
}
</script>

<style>
h1,
h2,
h3,
p {
  color: #f1f1e3;
}
.main {
  padding: 1em;
}
.input-field {
  background-color: #f1f1e3;
}
.input-field form {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0.5em;
}
.input-field form > div {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-right: 2em;
}
.input-field form p {
  margin: 0 0.5em;
  color: black;
}
.input-field form input {
  border: 1px solid;
  border-radius: 12px;
  padding: 0.3em;
  background-color: #f1f1e3;
}
.input-field form button {
  border: 1px solid;
  border-radius: 12px;
  padding: 0.3em;
  background-color: #1b1c4b;
  color: #f1f1e3;
}
.graph-container {
  display: flex;
  justify-content: center;
  margin-top: 1em;
}
.graph-container .graph h3 {
  text-align: center;
}
.graph-container .graph {
  width: 50%;
  margin: 0.4em;
  padding: 0.5em;
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 12px;
}
</style>
