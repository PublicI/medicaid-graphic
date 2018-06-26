<template>
    <div>
        <div id="chart"></div>
        

        <p class="note">Note: Spending for 2008 and 2009 is slightly understated because drug utilization data for Arizona's Medicaid program was not available.</p>

        <p class="source">Source: Center for Public Integrity Analysis of <a href="https://www.medicaid.gov/medicaid/prescription-drugs/state-drug-utilization-data/index.html" target="_top">Medicaid state drug utilization</a> data and <a href="https://www.medicaid.gov/medicaid/finance/state-expenditure-reporting/expenditure-reports/index.html" target="_top">Medicaid Budget and Expenditure System</a> data. Graphic by Julia Donheiser.</p>
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
            seriesI.name = "Total spending";
            seriesI.data = mydata.data[1];
            seriesI.pointStart = Date.UTC(2008,0,1); 
            seriesI.pointIntervalUnit = 'year'
            seriesI.marker = { symbol: 'circle' }; // , radius: 2
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
            colors: ['#17807E'],
            credits: {
                enabled: false
            },
            xAxis: {
                gridLineWidth: .5,
                endOnTick: true,
                max: Date.UTC(2016,0,1),
                plotLines: [
                    {
                        value: Date.UTC(2014),
                        width: 1,
                        color: '#bebebe',
                        zIndex: 3,
                        label: {
                            text: 'Medicaid expansion',
                            y: 110,
                            style: {
                                fontSize: '13px',
                                color: '#737373'
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
                    style: {
                        fontSize: '12px',
                        color: '#888'
                    },
                    formatter() {
                        return `$${Highcharts.numberFormat(this.value, 0)}`;
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
                pointFormat: '{series.name}:<b>${point.y:.1f} trillion</b>',
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
                text: '<b>Drug costs are on the rise...</b>',
                align: 'left',
                // margin: 20,
                x: 40,
                style: {
                    color: '#666'
                }
            },
            subtitle: {
                text: 'State and federal drug spending per Medicaid enrollee',
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
.note {
    line-height:120%;
    font-size:70%;
    color:#666;
    margin-bottom: 10px;
    padding-left: 40px;
}
.source {
    line-height:120%;
    font-size:70%;
    color:#666;
    padding-left: 40px;
}
</style>