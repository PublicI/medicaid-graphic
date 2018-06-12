<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: Center for Public Integrity analysis of Medicaid Budget and Expenditure Report and State Expenditure Report data. Graphic by Julia Donheiser.</p>
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
            seriesI.name = "National Average";
            seriesI.data = mydata.data[0];
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
                x: -20,
                labels: {
                    style: {
                        fontSize: '14px'
                    }
                }
            },
            credits: {
                enabled: false
            },
            xAxis: {
                labels: {
                    style: {
                        fontSize: '11px'
                    }
                },
                type: 'datetime'
            },
            yAxis: {
                title: {
                    text: 'Percentage of state and federal spending towards Medicaid'
                },
                labels: {
                    formatter() {
                        return `${Highcharts.numberFormat(this.value, 0)}%`;
                    },
                    style: {
                        fontSize: '11px'
                    }
                },
                plotLines: [
                    {
                        value: 2,
                        width: 1.5,
                        color: '#bebebe'
                    }
                ]
            },
            tooltip: {
                pointFormat: '{series.name}:<b>{point.y:.1f}%</b>',
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
                text: '<b>Medicaid spending is on the rise</b>'
            },
            subtitle: {
                text: 'Since 2008, the percentage of total state and federal spending going towards Medicaid has steadily increased.',
                style: {
                    fontSize: '15px'
                }
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