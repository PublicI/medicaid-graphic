<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: The National Institute on Money in Politics and the National Conference of State Legislatures. Graphic by Julia Donheiser.</p>
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
            series.name = 'ratios';
            series.data = mydata.data;
            tar.push(series);
            return tar;
        }
    },
    mounted() {
        Highcharts.chart('chart', {
            chart: {
                type: 'bar',
                style: {
                    fontFamily: 'tablet-gothic-narrow'
                }
            },
            colors: ['#17807E'],
            xAxis: {
                categories: data.states,
                tickLength: 0,
                margin: 0,
                align: 'right',
                title: {
                    text: null
                },
                labels: {
                    style: {
                        fontSize: '10px',
                        color: '#4c4c4c',
                    }
                }
            },
            yAxis: {
                min: 0,
                max: 3,
                gridLineWidth: 0,
                labels: {
                    enabled: false
                },
                title: {
                    text: null
                }
            },
            legend: {
                enabled: false
            },
            tooltip: {
                enabled: false
            },
            credits: {
                enabled: false
            },
            title: {
                text: '<b>Registered pharma lobbyists per state legislator</b>',
                align: 'left',
                fontSize: '20px',
                x: 90
            },
            style: {
                fontFamily: 'tablet-gothic-narrow'
            },
            plotOptions: {
                series: {
                    pointWidth: 10,
                    pointPadding: 20
                }
            },
            series: this.makeSeries(data)
        });

    }
};


</script>

<style scoped>
#chart {
    height: 900px;
    margin: 0 auto;
    padding-bottom: 15px;
}
.title {

}
.source {
    line-height:120%;
    font-size:80%;
    color:#666;
}
</style>