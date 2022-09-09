<template>
     <div class="container">
      <div class="topBar">
        <h1 class="heading">Load Profiles</h1>
        <p class="subTitle">Available datasets</p>
      </div>
      <div class="section">
        <div class="listHeader"></div>
        <div class="chartContainer">
        </div>
      </div>
     </div>
</template>

<script>
// @ is an alias to /src
// <LineChart :data="lineGraphData" :options="lineGraphOptions"/>

// import { defineComponent } from 'vue';
// import { LineChart } from 'vue-chart-3';

export default {
  name: 'Home',
  components: {
    //LineChart,
  }
}
</script>

<style>
  .container {
    background-color: #102D4C;
    height: 100%,
  }

  .topBar {
    display: flex;
    flex-direction: row;
    align-items: flex-end;
  }

  .heading{
    font-weight: bold;
    color: #fff;
    font-size: 38px;
  }

  .subTitle{
    color: #fff;
    font-size: 18px;
    margin-left: 10px;
  }

</style>