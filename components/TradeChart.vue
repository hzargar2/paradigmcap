<script setup>

import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

let props = defineProps(['trades']);


const chartData = computed(() => {
    console.log("computed", props.trades);
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
                    if (row['side'] === 'SELL') {
                        return -1 * row['quantity'];
                    }else{
                        return row['quantity'];
                    }
                }),
                yAxisID: 'y'
            }
        ]
    }
})

const chartOptions = computed(() => {
    return {
        responsive: true
    }
})





//
// new Chart(
//     document.getElementById("#trade_chart"),
//     {
//         options: {
//             plugins: {
//                 legend: {
//                     labels: {
//                         color: '#9aa5ce'
//                     }
//                 }
//             },
//             scales: {
//                 x: {
//                     ticks: {
//                         color: '#9aa5ce'
//                     },
//                     title: {
//                         color: '#9aa5ce',
//                         display: true,
//                         text: "Transaction Date",
//                         font: {
//                             size: 14
//                         },
//                         padding: 12
//                     },
//                     grid: {
//                         color: '#9aa5ce'
//                     },
//                 },
//                 y: {
//                     ticks: {
//                         color: '#9aa5ce'
//                     },
//                     type: 'logarithmic',
//                     title: {
//                         color: '#9aa5ce',
//                         display: true,
//                         text: "Shares Owned",
//                         font: {
//                             size: 14
//                         },
//                         padding: 12
//                     },
//                     max: 460000000,
//                     grid: {
//                         color: '#9aa5ce'
//                     },
//                 },
//                 y1: {
//                     ticks: {
//                         color: '#9aa5ce'
//                     },
//                     type: 'logarithmic',
//                     title: {
//                         color: '#9aa5ce',
//                         display: true,
//                         text: "Price Per Share",
//                         font: {
//                             size: 14
//                         },
//                         padding: 12
//                     },
//                     position: 'right',
//                     // grid line settings
//                     grid: {
//                         color: '#9aa5ce',
//                         drawOnChartArea: false, // only want the grid lines for one axis to show up
//                     },
//                 }
//             }
//         },
//         type: 'line',
//         data: {
//             labels: sorted_trades.map(row => row['date']),
//             datasets: [
//                 {
//                     label: 'Quantity (# shares)',
//                     data: sorted_trades.map(row => row['quantity']),
//                     yAxisID: 'y'
//                 }
//             ]
//         }
//     }
// );

</script>


<template>
    <Bar
        id="trade_chart"
        :options="chartOptions"
        :data="chartData"
    />
</template>

