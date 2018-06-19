<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: NEED SOURCE. Graphic by Julia Donheiser.</p>
    </div>
</template>

<script>
/* eslint-disable */
import data from '~/assets/lobbyist-ratios.json';
import Highcharts from 'highcharts';
import postal from 'journalize';
export default {
    methods: {
        makeSeries(mydata) {
            const tar = [];
            let series = {};
            series.data = mydata.data;
            tar.push(series);
            return series;
        }
    },
    mounted() {
        let states = data.states;
        let dSeries = this.makeSeries(data);
        console.log(dSeries);
        console.log(states);
        Highcharts.chart('chart', {
            chart: {
                type: 'bar'
            },
            xAxis: {
                categories: states,
                title: {
                    text: null
                }
            },
            yAxis: {
                data: dSeries.ratio,
                min: 0,
                title: {
                    text: null
                }
            },
            legend: {
                enabled: false
            },
        });

    }
};

</script>

<style scoped>
#chart {
    height: 1000px;
    margin: 0 auto;
    padding-bottom: 20px;
}
.source {
    line-height:120%;
    font-size:80%;
    color:#666;
}
</style>