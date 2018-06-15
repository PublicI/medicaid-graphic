<template>
    <div class="statebinContainer">

        <p class="title">How pharmaceutical companies influence Medicaid drug access</p>

        <p class="subtitle">Nearly 300 doctors sitting on state drug utilization boards from 2016-18 received payments from pharmaceutical companies </p>

        <svg style="width:100%;height:60px">
            <g class="legendLog" transform="translate(7,0)">
            </g>
        </svg>

        <div class="statebins">
            <div :style="'top:' + bin.y + 'px;left:' + bin.x + 'px;background-color:' + bin.color" class="statebin" v-for="bin in bins" v-tooltip="{ content: '<b>' + bin.state + '</b><br>' + bin.doctors + ' doctors<br>' + '$' + bin.payments }">
                {{bin.abbrev}}
            </div>
        </div>

        <p class="source">Source: Open Payments and NPPES.</p>
    </div>
</template>

<script>
import stats from '~/assets/doctor-payments.csv';
import { intcomma, postal } from 'journalize';
import * as d3 from 'd3';
import { legendColor } from 'd3-svg-legend';

export default {
    data() {
        return {
            stats: stats.map(({ state, payments, doctors }) => {
                return {
                    state,
                    doctors: +doctors,
                    payments: +payments
                };
            }),
            grid: [
                '                                  ME',
                '                   WI          VT NH',
                '    WA ID MT ND MN IL MI    NY MA',
                '    OR NV WY SD IA IN OH PA NJ CT RI',
                '    CA UT CO NE MO KY WV VA MD DE',
                '       AZ NM KS AR TN NC SC',
                '             OK LA MS AL GA',
                '    HI AK    TX             FL'
            ]
        };
    },
    mounted() {
        this.$nextTick(() => {
            let legendLog = legendColor()
                .shapeWidth(55)
                .cells([1000, 5000, 10000, 50000, 100000])
                .labelFormat(d3.format('$' + ',.0f'))
                .orient('horizontal')
                .scale(this.scale());

            d3.select('.legendLog').call(legendLog);
        });
    },
    methods: {
        scale() {
            return d3.scaleLog()
                .domain([0.1, 1000, 5000, 10000, 50000])
                .range(['#A8A8A8', '#7DBFE6', '#51AADE', '#3D7FA6', '#28556F']);
        }
    },
    computed: {
        bins() {
            let scale = this.scale();

            let binsRef = {};
            let bins = [];

            let boxSize = 26;

            let re = /\w+/g;

            this.grid.forEach(function(line, i) {
                let m;
                while ((m = re.exec(line))) { // eslint-disable-line no-cond-assign
                    let state = {
                        abbrev: m[0],
                        x: m.index / 3 * boxSize - (boxSize + 2),
                        y: i * boxSize,
                        color: null,
                        name: null,
                        percent: null,
                        number: null
                    };

                    bins.push(state);

                    binsRef[state.abbrev] = state;
                }
            });

            this.stats.forEach(function(d) {
                let abbrev = postal(d.state);
                if (abbrev in binsRef) {
                    binsRef[abbrev].color = scale(d.payments);
                    binsRef[abbrev].state = postal(d.state, true);
                    binsRef[abbrev].doctors = d.doctors;
                    binsRef[abbrev].payments = intcomma(Math.round(d.payments));
                }
            });

            return bins;
        }
    }
};
</script>

<style>
.statebins {
    position: relative;
    width: 300px;
    height: 220px;
    margin-top: -19px;
}

.statebin {
    position: absolute;
    width: 24px;
    height: 24px;
    background-color: #eee;
    color: white;
    text-align: center;
    font-size: 12px;
    line-height: 20px;
    padding-top: 1px;
}

.tooltip {
    display: block !important;
    z-index: 10000;
}

.tooltip .tooltip-inner {
    background: white;
    color: black;
    border-radius: 2px;
    padding: 5px 10px 4px;
    font-family: tablet-gothic-n2,tablet-gothic,Helvetica Neue,Helvetica,Arial,sans-serif;
    font-size: 16px;
    line-height: 19px;
}

.tooltip .tooltip-arrow {
    width: 0;
    height: 0;
    border-style: solid;
    position: absolute;
    margin: 5px;
    border-color: white;
    z-index: 1;
}

.tooltip[x-placement^="top"] {
    margin-bottom: 5px;
}

.tooltip[x-placement^="top"] .tooltip-arrow {
    border-width: 5px 5px 0 5px;
    border-left-color: transparent !important;
    border-right-color: transparent !important;
    border-bottom-color: transparent !important;
    bottom: -5px;
    left: calc(50% - 5px);
    margin-top: 0;
    margin-bottom: 0;
}

.tooltip[x-placement^="bottom"] {
    margin-top: 5px;
}

.tooltip[x-placement^="bottom"] .tooltip-arrow {
    border-width: 0 5px 5px 5px;
    border-left-color: transparent !important;
    border-right-color: transparent !important;
    border-top-color: transparent !important;
    top: -5px;
    left: calc(50% - 5px);
    margin-top: 0;
    margin-bottom: 0;
}

.tooltip[x-placement^="right"] {
    margin-left: 5px;
}

.tooltip[x-placement^="right"] .tooltip-arrow {
    border-width: 5px 5px 5px 0;
    border-left-color: transparent !important;
    border-top-color: transparent !important;
    border-bottom-color: transparent !important;
    left: -5px;
    top: calc(50% - 5px);
    margin-left: 0;
    margin-right: 0;
}

.tooltip[x-placement^="left"] {
    margin-right: 5px;
}

.tooltip[x-placement^="left"] .tooltip-arrow {
    border-width: 5px 0 5px 5px;
    border-top-color: transparent !important;
    border-right-color: transparent !important;
    border-bottom-color: transparent !important;
    right: -5px;
    top: calc(50% - 5px);
    margin-left: 0;
    margin-right: 0;
}

.tooltip.popover .popover-inner {
    background: #f9f9f9;
    color: black;
    padding: 24px;
    border-radius: 2px;
    box-shadow: 0 5px 30px rgba(black, .1);
}

.tooltip.popover .popover-arrow {
    border-color: #f9f9f9;
}

.tooltip[aria-hidden='true'] {
    visibility: hidden;
    opacity: 0;
    transition: opacity .15s, visibility .15s;
}

.tooltip[aria-hidden='false'] {
    visibility: visible;
    opacity: 1;
    transition: opacity .15s;
}
.source {
    font-size: 14px;
    line-height: 16px;
    color: #666;
    margin-bottom: 15px;
    margin-left: 7px;
}

.title {
    font-family: tablet-gothic-narrow,
        sans-serif;
    font-size: 20px;
    line-height: 22px;
    width: 300px;
    word-wrap: break-word;
    padding-bottom: 0px;
    border-bottom: 0px;
    margin-bottom: 8px;
    margin-left: 7px;
}

.subtitle {
    font-family: tablet-gothic-narrow,
        sans-serif;
    font-size: 15px;
    line-height: 17px;
    color: #666;
    margin-bottom: 15px;
    width: 300px;
    word-wrap: break-word;
    margin-left: 7px;
}

.legendLog .label {
    font-family: tablet-gothic-n2,tablet-gothic,Helvetica Neue,Helvetica,Arial,sans-serif;
    font-size: 13px;
    line-height: 16px;
    fill: rgb(100,100,100);

}
</style>
