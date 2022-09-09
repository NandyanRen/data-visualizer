<template>
  <div class="container">
    <div class="topBar">
      <h1>Load Profiles</h1>
      <p>Available datasets</p>
    </div>
    <div class="sectionContainer">
      <div class="profileList">
        <h6>Name</h6>
        <hr />
        <div
          v-for="profile in profiles"
          :key="profile">
              <div>
                  <h2>{{profile.name}}</h2>
              </div>
              <div>
                  <p>{{profile.value +' MWh'}}</p>
                  <hr />
              </div>
            </div>
      </div>
      <div class="chartContainer">
        <canvas id="myChart"></canvas>
      </div>
    </div>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';
import sampleData from '../data/sample-dataset.json'
import profiles from '../data/load-profiles.json'


export default {
  name: 'Dashboard',
  props: {
    msg: String
  },

  data(){
    return {
      sampleData,
      days: sampleData.chunked_time_series.days,
      profiles,
    }
  },
  
  methods: {
    getDataset(data, color) { 
      return { 
        data: data,
        fill: false,
        borderColor: color,
        tension: 0.1,
        pointRadius: 0,
      };
    },
    getAverage(i, days) {
      let sum = 0;
      days.forEach(day => {
        sum += day.values[i];
      });
      return sum / days[i].values.length;
    }
  },

  mounted(){
    let i = 0;
    let lineChartData = [];
    let dataAverage = [];
    const ctx = document.getElementById('myChart');

    // ## Missing Time Format For X-Axis label
    const labels = this.sampleData.chunked_time_series.x_axis;

    lineChartData["datasets"] = []

    for(i = 0; i < this.days[0].values.length; i++){
      dataAverage.push(this.getAverage(i, this.days)/8);
    }

    // ## This shows a pretty high average and still needs fixing but the trend is similar to example
    // lineChartData.push(this.getDataset(dataAverage, 'rgb(16,45,76)'));

    this.days.forEach(day => {
      lineChartData.push(this.getDataset(day.values, 'rgb(198,198,198)'));
    });

    const data = {
      labels: labels,
      datasets: lineChartData
    }

const myChart = new Chart(ctx, {
  type: 'line',
  data: data,
  options: {
      plugins: {
         legend: {
            display: false
         }
      },
      scales: {
      x: {
        grid: {
            display: false
            }
          },
        }
      }
  });

myChart;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  background-color: #102D4C;
  padding: 20px;
  font-family: Helvetica;
}

.topBar {
  color: #ffffff;
  display: flex;
  flex-direction: row;
  align-items: baseline;
  gap: 20px
}

.sectionContainer{
  display: flex;
  flex-direction: row;
  gap: 5px;
  text-align: left;
}

.profileList {
  background-color: #F1F1F1;
}

.profileList h2 {
  font-size: 14px;
  font-weight: bold;
  color: #2B3E52;
  padding: 0px 20px 0px 20px;
}

.profileList h6 {
  font-size: 10px;
  font-weight: bold;
  color: #2B3E52;
  padding: 0px 20px 0px 20px;

}

hr {
  border-top: 2px solid white;
}

.profileList p {
  font-size: 12px;
  color: #2B3E52;
  padding-left: 20px;

}

.chartContainer {
  background-color: #F1F1F1;
  position: relative;
  height:100%;
  width:80vw;
}

</style>
