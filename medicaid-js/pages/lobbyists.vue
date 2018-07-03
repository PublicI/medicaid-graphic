<template>
    <div>
        <div id="chart"></div>
        
        <p class="source">Source: Center for Public Integrity analysis of 2017 data from the <a href="https://www.followthemoney.org/" target="_top">National Institute on Money in Politics</a>. Graphic by Julia Donheiser.</p>
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
                    reserveSpace: true,
                    allowOverlap: true,
                    step: 1,
                    style: {
                        fontSize: '12.5px',
                        color: '#383838',
                    }
                }
            },
            yAxis: {
                min: 0,
                max: 180,
                gridLineWidth: 0,
                title: {
                    text: null
                },
                labels: {
                    enabled: false,
                },
                plotLines: [
                    {
                        value: 50,
                        width: 1,
                        color: '#bebebe',
                        label: {
                            y: 230,
                            text: '50 lobbyists',
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                                       
                    },
                    {
                        value: 100,
                        width: 1,
                        color: '#bebebe',
                        label: {
                            y: 80,
                            text: '100 lobbyists',
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                                       
                    },
                    {
                        value: 150,
                        width: 1,
                        color: '#bebebe',
                        label: {
                            y: 20,
                            text: '150 lobbyists',
                            style: {
                                color: '#737373',
                                fontSize: '13px'
                            }
                        }                                       
                    }               
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
                text: '<b>Registered Pharma lobbyists by state</b>',
                align: 'left',
                style: {
                    // fontSize: '20px',
                    color: '#666'
                },
            },
            style: {
                fontFamily: 'tablet-gothic-narrow'
            },
            plotOptions: {
                series: {
                    pointWidth: 11,
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
    height: 750px;
    margin: 0 auto;
    padding-bottom: 15px;
}

.title {

}

.source {
    line-height:120%;
    font-size:70%;
    color:#666;
}

</style>