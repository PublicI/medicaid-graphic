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
            for (let i = 0; i < mydata.index.length; i++) {
                let seriesI = {};
                seriesI.name = mydata.index[0];
                seriesI.data = mydata.data[0];
                tar.push(seriesI);
            }
            return tar;
        }
    },

    mounted() {
        Highcharts.chart('chart', {
            chart: {
                type: 'bar'
            },
            xAxis: {
                categories: data.states,
                title: {
                    text: null
                }
            },
            yAxis: {
                data: data.index[1],
                min: 0,
                title: {
                    text: null
                }
            },
            legend: {
                enabled: false
            },
            series: this.makeSeries(data)
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