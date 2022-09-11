<template>
    <div class="chartContainer">
          <canvas id="myChart" style="maintainAspectRatio: true;"></canvas>
    </div>
</template>

<script>
    import sampleData from '../data/sample-dataset.json'
    import Chart from 'chart.js/auto';
    export default {
        name: 'ChartSection',
        data(){
            return {
                xLabel: sampleData.chunked_time_series.x_axis,
                days: sampleData.chunked_time_series.days,
                dataAverage : [],
                lineChartData : [],
            }
        },
        methods: {
            getDataset(data, color, lineWidth) { 
                return { 
                    data: data,
                    fill: false,
                    borderColor: color,
                    tension: 0.1,
                    pointRadius: 0,
                    borderWidth: lineWidth,
            };
            },
            getAverage(i, days) {
                let sum = 0;
                days.forEach(day => {
                    sum += day.values[i];
                });
                return sum / days.length;
            },
        },
        mounted(){
            let i = 0;
            this.lineChartData["datasets"] = []
            const ctx = document.getElementById('myChart');
            const labels = this.xLabel;

            for(i = 0; i < this.days[0].values.length; i++){
                this.dataAverage.push(this.getAverage(i, this.days));
            }
            this.lineChartData.push(this.getDataset(this.dataAverage, 'rgb(16,45,76)', 5));

            this.days.forEach(day => {
                this.lineChartData.push(this.getDataset(day.values, 'rgb(198,198,198)', .3));
            });

            const data = {
                labels: labels,
                datasets: this.lineChartData
            }

            const myChart = new Chart(ctx, {
            type: 'line',
            data: data,
            options: {
                plugins: {
                    legend: {
                        display: false,
                    }
                },
                scales: {
                    x: {
                        grid: {
                            display: false
                        },
                        ticks: {
                            autoSkip: true,
                            maxTicksLimit: 10,
                            callback: function(value){
                                let hours, counter = 0;
                                const xLabel = parseInt(this.getLabelForValue(value));
                                counter = xLabel / 30;
                                hours = counter / 2;
                                if(counter % 2 != 0){
                                    return Math.floor(hours) + ':' + '30';
                                }
                                return hours + ':' + '00';
                            }
                        }
                    },
                    y: {
                        beginAtZero: true,
                        ticks: {
                            stepSize: 2500,
                            callback: function(value){
                                return parseInt(value) / 1000 + 'k';
                            },
                        }
                    },
                }
            }
        });
        myChart;
        }
    }
</script>

<style scoped>
    .chartContainer {
        position: relative;
        height: 100%;
        width:75vw;
        padding: 25px;
    }
</style>