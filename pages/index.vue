<template>
  <div class="container">
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
    <Chart v-if="show" :get-location="location" :get-period="period" />
    <p>Author: broccolingual</p>
  </div>
</template>

<script>
import Chart from './Chart'

export default {
  components: {
    Chart,
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
p {
  color: #f1f1e3;
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
}
.input-field form input {
  background-color: #1b1c4b;
  border: 1px solid #f1f1e3;
  border-radius: 12px;
  padding: 0.2em;
  color: #f1f1e3;
}
</style>
