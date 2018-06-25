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
            colors: ['#51AADE'],
            credits: {
                enabled: false
            },
            plotOptions: {
                series: {
                    lineWidth: 3,
                    marker: {
                        radius: 3,
                        lineWidth: 1
                    }
                }
            },
            xAxis: {
                gridLineWidth: .5,
                max: Date.UTC(2017,0,1),
                endOnTick: true,
                plotLines: [
                    {
                        value: Date.UTC(2014),
                        width: 1,
                        color: '#bebebe',
                        zIndex: 3,
                        label: {
                            text: 'Medicaid expansion',
                            y: 100,
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                        
                    }                    
                ],
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
                text: '<b>Medicaid spending is on the rise...</b>',
                align: 'left',
                x: 40,
                // margin: 20,
                style: {
                    color: '#666'
                }
            },
            subtitle: {
                text: 'Percentage of total state and federal spending that went towards Medicaid',
                align: 'left',
                x: 40,
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
    padding-left: 40px;
}
</style>