<template>
  <div id="chart">
    <div class="top-container">
      <div class="status-container water-container">
        Today Water Temperature: {{ temperature }} °C
      </div>
      <div class="status-container">
        Status:
        <div
          class="mort-container"
          :class="[{ ' alert': mortVal > 40 }, { ' safe': mortVal < 20 }]"
        ></div>
      </div>
    </div>
    <!-- <div class="toolbar">
      <button
        id="one_month"
        @click="updateData('one_month')"
        :class="{ active: selection === 'one_month' }"
      >
        1M
      </button>

      <button
        id="six_months"
        @click="updateData('six_months')"
        :class="{ active: selection === 'six_months' }"
      >
        6M
      </button>

      <button
        id="one_year"
        @click="updateData('one_year')"
        :class="{ active: selection === 'one_year' }"
      >
        1Y
      </button>

      <button id="ytd" @click="updateData('ytd')" :class="{ active: selection === 'ytd' }">
        YTD
      </button>

      <button id="all" @click="updateData('all')" :class="{ active: selection === 'all' }">
        ALL
      </button>
    </div> -->

    <div id="chart-timeline" class="chart-container">
      <apexchart
        type="area"
        height="100%"
        width="100%"
        ref="chart"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </div>
    <div class="container">
      <div
        class="temperature-container"
        :class="[{ ' alert': mortVal > 40 }, { ' safe': mortVal < 20 }]"
      >
        <p class="temperature-font">SGR: {{ sgrVal }} %</p>
      </div>
      <div
        class="temperature-container"
        :class="[{ ' alert': mortVal > 40 }, { ' safe': mortVal < 20 }]"
      >
        <p class="temperature-font">FCR: {{ fcrVal }}</p>
      </div>
      <div
        class="temperature-container"
        :class="[{ ' alert': mortVal > 40 }, { ' safe': mortVal < 20 }]"
      >
        <p class="temperature-font">SFR: {{ sfrVal }} %</p>
      </div>
      <div
        class="temperature-container"
        :class="[{ ' alert': mortVal > 40 }, { ' safe': mortVal < 20 }]"
      >
        <p class="temperature-font">MORT: {{ mortVal }} %</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import ApexCharts from 'apexcharts'

const defaultTemperature = 22
const temperature = ref(defaultTemperature)

const toDigit = (value) => {
  return Math.round(value * 10) / 10
}

const sgrVal = computed(() => {
  return toDigit(10 - (temperature.value - defaultTemperature) * 1)
})
const fcrVal = computed(() => {
  return toDigit(1.8 + (temperature.value - defaultTemperature) * 0.1)
})
const sfrVal = computed(() => {
  return toDigit(3 - (temperature.value - defaultTemperature) * 0.25)
})
const mortVal = computed(() => {
  return toDigit(5 + (temperature.value - defaultTemperature) * 5)
})

const series = ref([
  {
    data: [
      [new Date('2024-07-01').getTime(), 20.9],
      [new Date('2024-07-02').getTime(), 21.3],
      [new Date('2024-07-03').getTime(), 21.7],
      [new Date('2024-07-04').getTime(), 22.1],
      [new Date('2024-07-05').getTime(), 22.5],
      [new Date('2024-07-06').getTime(), 23.0],
      [new Date('2024-07-07').getTime(), 29.2],
      [new Date('2024-07-08').getTime(), 28.3],
      [new Date('2024-07-09').getTime(), 27.9],
      [new Date('2024-07-10').getTime(), 26.9],
      [new Date('2024-07-11').getTime(), 23.3],
      [new Date('2024-07-12').getTime(), 22.1],
      [new Date('2024-07-13').getTime(), 23.7],
      [new Date('2024-07-14').getTime(), 24.2],
      [new Date('2024-07-15').getTime(), 26.4],
      [new Date('2024-07-16').getTime(), 28.4],
      [new Date('2024-07-17').getTime(), 29.5],
      [new Date('2024-07-18').getTime(), 32.9],
      [new Date('2024-07-19').getTime(), 32.8],
      [new Date('2024-07-20').getTime(), 32.5],
      [new Date('2024-07-21').getTime(), 32.3],
      [new Date('2024-07-22').getTime(), 30.0],
      [new Date('2024-07-23').getTime(), 30.4],
      [new Date('2024-07-24').getTime(), 30.3],
      [new Date('2024-07-25').getTime(), 30.3],
      [new Date('2024-07-26').getTime(), 30.9],
      [new Date('2024-07-27').getTime(), 31.1],
      [new Date('2024-07-28').getTime(), 31.7],
      [new Date('2024-07-29').getTime(), 31.2],
      [new Date('2024-07-30').getTime(), 31.0],
      [new Date('2024-07-31').getTime(), 30.4]
    ]
  }
])

const chartOptions = ref({
  chart: {
    id: 'area-datetime',
    type: 'area',
    height: 350,
    zoom: {
      autoScaleYaxis: true
    }
  },
  annotations: {
    // yaxis: [
    //   {
    //     y: 30,
    //     borderColor: '#999',
    //     label: {
    //       show: true,
    //       text: 'Support',
    //       style: {
    //         color: '#fff',
    //         background: '#00E396'
    //       }
    //     }
    //   }
    // ],
    // xaxis: [
    //   {
    //     x: new Date('14 Nov 2012').getTime(),
    //     borderColor: '#999',
    //     yAxisIndex: 0,
    //     label: {
    //       show: true,
    //       text: 'Rally',
    //       style: {
    //         color: '#fff',
    //         background: '#775DD0'
    //       }
    //     }
    //   }
    // ]
  },
  dataLabels: {
    enabled: false
  },
  markers: {
    size: 0,
    style: 'hollow'
  },
  xaxis: {
    type: 'datetime',
    min: new Date('2024-07-01').getTime(),
    max: new Date('2024-07-31').getTime()
  },
  tooltip: {
    enabled: true,
    theme: 'dark',
    style: {
      fontSize: '13px'
    },
    x: {
      format: 'dd MM yyyy'
    },
    y: {
      formatter: (val, { dataPointIndex, w }) => {
        temperature.value = val
        return val
      }
    }
  },

  fill: {
    type: 'gradient',
    gradient: {
      shadeIntensity: 1,
      opacityFrom: 0.7,
      opacityTo: 0.9,
      stops: [0, 100]
    }
  }
})
</script>

<style scoped>
.water-container {
  border: gray solid;
  border-radius: 8px;
  padding: 5px 8px;
  font-size: 10px;
}

.temperature-container {
  border: gray solid;
  border-radius: 8px;
  padding: 5px 8px;
  font-size: 10px;
}

.container {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.alert {
  background-color: red !important;
}

.safe {
  background-color: green !important;
}

.mort-container {
  width: 50px;
  height: 50px;
  border-radius: 100px;
  padding: 20px 20px;
  margin-left: 10px;
  background-color: yellow;
}

.status-container {
  display: flex;
  align-items: center;
  margin-bottom: 30px;
  margin-left: 50px;
  font-size: 10px;
}

.top-container {
  display: flex;
}

@media (min-width: 600px) {
  .chart-container,
  .container {
    width: 600px;
  }

  .temperature-container {
    border-radius: 10px;
    padding: 10px 20px;
    background-color: yellow;
  }

  .water-container {
    border-radius: 10px;
    padding: 10px 20px;
  }

  .temperature-container,
  .status-container {
    font-size: 18px;
  }
}

@media (min-width: 900px) {
  .chart-container,
  .container {
    width: 900px;
  }

  .temperature-container,
  .status-container,
  .container {
    font-size: 20px;
  }
}

@media (min-width: 1200px) {
  .chart-container,
  .container {
    width: 1200px;
  }

  .temperature-container,
  .status-container {
    font-size: 22px;
  }
}

@media (min-width: 1500px) {
  .chart-container,
  .container {
    width: 1500px;
  }

  .temperature-container,
  .status-container {
    font-size: 26px;
  }
}

@media (min-width: 1800px) {
  .chart-container,
  .container {
    width: 1800px;
  }

  .temperature-container,
  .status-container {
    font-size: 30px;
  }
}

@media (min-height: 300px) {
  .chart-container {
    height: 250px;
  }
}

@media (min-height: 600px) {
  .chart-container {
    height: 400px;
  }
}

@media (min-height: 900px) {
  .chart-container {
    height: 600px;
  }
}

@media (min-height: 1200px) {
  .chart-container {
    height: 900px;
  }
}

.temperature-font {
  color: black;
}
</style>
