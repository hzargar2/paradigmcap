<script setup>

import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

let props = defineProps(['trades']);

const chartData = computed(() => {
    return {
        labels: props.trades.sort((a, b) => {
            if (a['date'] && b['date']) {
                return new Date(a['date']) - new Date(b['date']);
            }
        }).map(row => row['date']),
        datasets: [
            {
                label: 'Quantity (# shares)',
                data: props.trades.sort((a, b) => {
                    if (a['date'] && b['date']) {
                        return new Date(a['date']) - new Date(b['date']);
                    }
                }).map(row => {
                    if (row['side'].includes('SELL')) {
                        return -1 * row['quantity'];
                    }else{
                        return row['quantity'];
                    }
                }),
                yAxisID: 'y',
                backgroundColor: props.trades.map((trade) => {
                    if (trade['side'].includes('SELL')) {
                        return 'red';
                    }else{
                        return 'green'
                    }
                }),
            }
        ]
    }
})

const chartOptions = computed(() => {
    return {
        plugins: {
            zoom: {
                zoom: {
                    wheel: {
                        enabled: true,
                    },
                    pinch: {
                        enabled: true
                    },
                    mode: 'xy',
                }
            },
            title: {
                text: "Trades over Time",
                display: true,
                color: '#000000',
                font: {
                    size: 16,
                    weight: 'normal'
                },
                padding: 18
            },
            legend: {
                display: false
            }
        },
        scales: {
            x: {
                title: {
                    display: true,
                    text: "Transaction Date",
                    color: '#000000',
                    font: {
                        size: 14
                    },
                    padding: 12
                }
            },
            y: {
                title: {
                    display: true,
                    text: "Quantity Transacted",
                    color: '#000000',
                    font: {
                        size: 14
                    },
                    padding: 12
                }
            }
        },
        responsive: true,
        maintainAspectRatio: false
    }
})

</script>


<template>
    <div class="flex w-full">
        <Bar
            id="trade_chart"
            :options="chartOptions"
            :data="chartData"
        />
    </div>

</template>

