<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: Center for Public Integrity analysis of Medicaid Budget and Expenditure Report, State Expenditure Report and State Drug Utilization data. Graphic by Julia Donheiser.</p>
    </div>
</template>

<script>
/* eslint-disable */
import data from '~/assets/indexed-means.json';
import Highcharts from 'highcharts';
export default {
    methods: {
        makeDates(floatArr) {
            let tar = [];
            for (let i = 0; i < floatArr.length; i++) {
                tar[i] = Date.UTC(floatArr[i],0,1);
            }
            return tar;
        },
        makeSeries(mydata) {
            const tar = [];
            let seriesI = {};
            seriesI.name = "Total Spending";
            seriesI.data = mydata.data[1];
            seriesI.pointStart = Date.UTC(2006,0,1); 
            seriesI.pointIntervalUnit = 'year'
            seriesI.marker = { symbol: 'circle', radius: 3 };
            tar.push(seriesI);
            return tar;
        }
    },
    mounted() {
        let cols = this.makeDates(data.columns)
        let dSeries = this.makeSeries(data);
        Highcharts.setOptions({
            chart: {
                style: {
                    fontFamily: 'tablet-gothic-narrow'
                }
            }
        });
        Highcharts.chart('chart', {
            colors: ['#6699ff'],
            title: {
                display: 'null',
                text: ' ',
                x: -20 // center
            },
            subtitle: {
                display: 'null',
                text: ' ',
                x: -20
            },
            credits: {
                enabled: false
            },
            xAxis: {
                labels: {
                    style: {
                        fontSize: '10px'
                    }
                },
                type: 'datetime'
            },
            yAxis: {
                title: {
                    text: 'Medicaid drug spending (Trillions, USD)'
                },
                labels: {
                    formatter() {
                        return `$${Highcharts.numberFormat(this.value, 0)}`;
                    }
                },
                plotLines: [
                    {
                        value: 0,
                        width: 1.5,
                        color: '#bebebe'
                    }
                ]
            },
            tooltip: {
                pointFormat: '{series.name}:<b>${point.y:.1f} trillion</b>',
                crosshairs: true
            },
            legend: {
                enabled: true,
                align: 'right',
                verticalAlign: 'bottom',
                borderWidth: 0
            },
            series: dSeries,
            title: {
                text: '<b>Drug spending under Medicaid is increasing</b>'
            },
            style: {
                fontFamily: 'tablet-gothic-narrow'
            }
        });
    }
};

</script>

<style scoped>
#chart {
    height: 400px;
    margin: 0 auto;
    padding-bottom: 20px;
}
.source {
    line-height:120%;
    font-size:80%;
    color:#666;
}
</style>