<script setup>

import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

let props = defineProps(['trades']);

let tickers = [...new Set(props.trades.map((value) => value['full_ticker']))];
let selected_ticker= ref(null);

let sides = [...new Set(props.trades.map((value) => value['side']))];
let selected_side= ref(null);

let commission_types = [...new Set(props.trades.map((value) => value['commission_type']))];
let selected_commission_type= ref(null);

let total_commission = props.trades.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0);

let average_commission = Math.round(((props.trades.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / props.trades.length) + Number.EPSILON) * 100) / 100

let series = ref([
    {
        name: "Quantity (Shares)",
        data: props.trades.sort((a, b) => {
            if (a['date'] && b['date']) {
                return new Date(a['date']) - new Date(b['date']);
            }
        }).map(row => {
            let point = {}

            point['x'] = row['date'];

            if (row['side'].includes('SELL')) {
                point['y'] = -1 * row['quantity'];
                point['fillColor'] = '#C23829';
                point['strokeColor'] = '#C23829';
            }else{
                point['y'] = row['quantity'];
                point['fillColor'] = '#1f993f';
                point['strokeColor'] = '#1f993f';
            }

            return point;

        }),
        color: "#1A56DB",
    },
]);

let chartOptions = ref({
        chart: {
            id: "trade-chart",
            type: "bar",
            fontFamily: "Inter, sans-serif",
            dropShadow: {
                enabled: false,
            },
            toolbar: {
                show: true,
            },
        },
        tooltip: {
            enabled: true,
            x: {
                show: true,
            },
        },
        fill: {
            type: "solid"
        },
        dataLabels: {
            enabled: false,
        },
        grid: {
            show: true,
            strokeDashArray: 4,
            padding: {
                left: 2,
                right: 2,
                top: 0
            },
        },
        xaxis: {
            categories: props.trades.sort((a, b) => {
                if (a['date'] && b['date']) {
                    return new Date(a['date']) - new Date(b['date']);
                }
            }).map(row => row['date']),
            labels: {
                show: true,
            },
            axisBorder: {
                show: true,
            },
            axisTicks: {
                show: true,
            },
        },
        yaxis: {
            show: true,
        },
    })

const filterTrades = (event) => {

    console.log(selected_ticker);

    let filtered_trades = props.trades;
    if (selected_side.value !== null){
        filtered_trades = filtered_trades.filter((trade) => trade['side'] === selected_side.value);
    }
    if (selected_ticker.value !== null){
        filtered_trades = filtered_trades.filter((trade) => trade['full_ticker'] === selected_ticker.value);
    }
    if (selected_commission_type.value !== null){
        filtered_trades = filtered_trades.filter((trade) => trade['commission_type'] === selected_commission_type.value);
    }

    chartOptions.value = {
        ...chartOptions.value,
        ...{
            xaxis: {
                categories: filtered_trades.sort((a, b) => {
                    if (a['date'] && b['date']) {
                        return new Date(a['date']) - new Date(b['date']);
                    }
                }).map(row => row['date']),
                labels: {
                    show: true,
                },
                axisBorder: {
                    show: true,
                },
                axisTicks: {
                    show: true,
                },
            },
        }}

    series.value = [
        {
            name: "Quantity (Shares)",
            data: filtered_trades.sort((a, b) => {
                if (a['date'] && b['date']) {
                    return new Date(a['date']) - new Date(b['date']);
                }
            }).map(row => {
                let point = {}

                point['x'] = row['date'];

                if (row['side'].includes('SELL')) {
                    point['y'] = -1 * row['quantity'];
                    point['fillColor'] = '#C23829';
                    point['strokeColor'] = '#C23829';
                }else{
                    point['y'] = row['quantity'];
                    point['fillColor'] = '#1f993f';
                    point['strokeColor'] = '#1f993f';
                }

                return point;

            }),
            color: "#1A56DB",
        }
    ]


    console.log(chartOptions.value)
    console.log(series.value)


}


</script>


<template>

    <div class="w-full bg-white rounded-lg shadow dark:bg-gray-800 p-4 md:p-6">
        <div class="flex justify-between">
            <div class="flex flex-row space-x-6">
                <form class="max-w-sm">
                    <select @change="filterTrades($event)" v-model="selected_ticker" id="selected_ticker" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                        <option selected :value="null">All Tickers</option>
                        <option v-for="item in tickers" :value="item">{{item}}</option>
                    </select>
                </form>

                <form class="max-w-sm">
                    <select @change="filterTrades($event)" v-model="selected_side" id="selected_side" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                        <option selected :value="null">All Sides</option>
                        <option v-for="item in sides" :value="item">{{item}}</option>
                    </select>
                </form>

                <form class="max-w-sm">
                    <select @change="filterTrades($event)" v-model="selected_commission_type" id="selected_commission_type" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                        <option selected :value="null">All Commission Types</option>
                        <option v-for="item in commission_types" :value="item">{{item}}</option>
                    </select>
                </form>
            </div>
        </div>
        <ClientOnly>
            <apexchart
                height="500px"
                :options="chartOptions"
                :series="series"
            ></apexchart>
        </ClientOnly>
    </div>

</template>

