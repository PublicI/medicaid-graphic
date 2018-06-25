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
import {postal, apstate} from 'journalize';
export default {

    methods: {
        makeSeries(mydata) {
            const tar = [];
            let series = {};
            series.name = 'ratios';
            series.data = mydata.data;
            tar.push(series);
            return tar;
        },
        apStyle(state) {
            return apstate(postal(state, true));
        }
    },
    mounted() {
        Highcharts.chart('chart', {
            chart: {
                type: 'bar',
                paddingLeft: -10,
                style: {
                    fontFamily: 'tablet-gothic-narrow'
                }
            },
            xAxis: {
                categories: data.states.map(this.apStyle),
                tickLength: 0,
                align: 'right',
                title: {
                    text: null
                },
                labels: {
                    style: {
                        fontSize: '11px',
                        color: '#383838',
                    }
                }
            },
            yAxis: {
                min: 0,
                max: 2.2,
                gridLineWidth: 0,
                title: {
                    text: null
                },
                labels: {
                    enabled: false,
                },
                plotLines: [
                    {
                        value: 1,
                        width: 1,
                        color: '#bebebe',
                        label: {
                            y: 80,
                            text: '1 lobbyist per legislator',
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                        
                    },
                    {
                        value: 2,
                        width: 1,
                        color: '#bebebe',
                        label: {
                            y: 40,
                            text: '2 lobbyists per legislator',
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                        
                    } /*,
                    {
                        value: 3,
                        width: 1.5,
                        color: '#bebebe',
                        label: {
                            align: 'left',
                            text: '3 lobbyists per legislator',
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                        
                    }   */                
                ]
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
                style: {
                    // fontSize: '20px',
                    color: '#666'
                },
                x: 40
            },
            style: {
                fontFamily: 'tablet-gothic-narrow'
            },
            plotOptions: {
                series: {
                    pointWidth: 10,
                    pointPadding: 20,
                    color: '#17807E',
                    states: {
                        hover: {
                            enabled: false
                        }
                    }
                }
            },
            series: this.makeSeries(data)
        });

    },
    computed: {
        stateNames() {
            let ret = [];
            this.data.forEach(function(d) {
                let temp = postal(d.states);
                ret.push(temp);
            });
            return ret;
        }
    }
};


</script>

<style scoped>
#chart {
    height: 800px;
    margin: 0 auto;
    padding-bottom: 15px;
}
.title {

}
.source {
    line-height:120%;
    font-size:70%;
    color:#666;
    padding-left: 40px;
}
</style>