<template>
  <div id="index">
    <v-card class="pa-6">
      <GChart
        type="LineChart"
        :data="chartData"
        :options="chartOptions"
      />
      <v-row class="headline justify-center">
        <span class="px-6"> p: {{ p }} </span>
        <span class="px-6"> q: {{ q }} </span>
        <span class="px-6"> Mode: {{ (p-1)/(p+q-2) }} </span>
      </v-row>
      <v-row>
        <v-col cols=6>
          <v-slider
              v-model="p"
              label="p"
              class="align-center"
              :max="10"
              :min="1"
              hide-details
          />
        </v-col>
        <v-col cols=6>
          <v-slider
              v-model="q"
              label="q"
              class="align-center"
              :max="10"
              :min="1"
              hide-details
          />
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script>
import { GChart } from 'vue-google-charts'
var { jStat } = require('jstat')

// import { GoogleCharts } from 'google-charts'
// GoogleCharts.load('current', { packages: ['corechart'] })
const transpose = a => a[0].map((_, c) => a.map(r => r[c]));

export default {
  components: {
    GChart
  },
  data () {
    return {
      header: ['index', 'value'],
      x: Array.from(Array(11), (_, k) => k),
      p: 8,
      q: 2,
      chartOptions: {
        curveType: 'function',
        height: 400,
        axis: {gridlines: {color: 'none'}},
        legend: { position: 'none' }
      }
    }
  },
  computed: {
    y() {
      return jStat( 0, 1, 11, x => jStat.beta.pdf( x, this.p, this.q ))[0]
    },
    rawData() {
      console.log([this.x, this.y])
      return [this.x, this.y]
    },
    chartData() {
      return [this.header].concat(transpose(this.rawData))
    }
  },
  created() {
    console.log(transpose(this.rawData))
  }
}
</script>
