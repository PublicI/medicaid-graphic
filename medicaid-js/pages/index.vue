<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: Center for Public Integrity analysis of data collected by the <a href="https://www.nasbo.org/reports-data/state-expenditure-report" target="_top">National Association of State Budget Officers</a>. Graphic by Julia Donheiser.</p>
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
            seriesI.name = "National average";
            seriesI.data = mydata.data[0];
            seriesI.pointStart = Date.UTC(2008,0,1); 
            seriesI.pointIntervalUnit = 'year'
            seriesI.marker = { symbol: 'circle'}; //  radius: 2 
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
            chart: {
                marginLeft: 50
            },
            colors: ['#3D7FA6'],
            credits: {
                enabled: false
            },
            plotOptions: {
                series: {
                    lineWidth: 3,
                    marker: {
                        radius: 3,
                        lineWidth: 1
                    },
                    states: {
                        hover: {
                            enabled: false
                        }
                    }
                }
            },
            xAxis: {
                gridLineWidth: .5,
                max: Date.UTC(2016,0,1),
                endOnTick: true,
                labels: {
                    style: {
                        fontSize: '12px',
                        color: '#888'
                    }
                },
                type: 'datetime'
            },
            yAxis: {
                title: {
                    text: ''
                },
                labels: {
                    formatter() {
                        return `${Highcharts.numberFormat(this.value, 0)}%`;
                    },
                    style: {
                        fontSize: '12px',
                        color: '#888'
                    }
                },
                plotLines: [
                    {
                        width: .5,
                        color: '#bebebe'
                    }
                ]
            },
            tooltip: {
                pointFormat: '{series.name}:<b>{point.y:.1f}%</b>',
                crosshairs: false,
                enabled: false
            },
            legend: {
                enabled: false,
                align: 'right',
                verticalAlign: 'bottom',
                borderWidth: 0
            },
            series: dSeries,
            title: {
                text: '<b>... and Medicaid spending is increasing</b>',
                align: 'left',
                style: {
                    color: '#666'
                }
            },
            subtitle: {
                text: 'Percentage of total state spending on Medicaid, including federal assistance',
                align: 'left',
                style: {
                    fontSize: '15px',
                    color: '#666'
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
    font-size:70%;
    color:#666;
}
</style>