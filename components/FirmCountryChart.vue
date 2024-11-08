<script setup>

import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';
import { Pie } from 'vue-chartjs';

ChartJS.register(ArcElement, Tooltip, Legend)

let props = defineProps(['countries']);
let country_counts = {};

for (const country of props.countries) {
    country_counts[country] = country_counts[country] ? country_counts[country] + 1 : 1;
}

let sum_of_counts = Object.values(country_counts).reduce((pv, cv) => { return pv + cv; }, 0);

console.log(sum_of_counts)
console.log(Object.values(country_counts).map((val) => {
    return val/sum_of_counts * 100;
}))
console.log(country_counts);
console.log(props.countries)
console.log(Object.values(country_counts));
const chartData = computed(() => {
    return {
        labels: Object.keys(country_counts),
        datasets: [
            {
                backgroundColor: Object.values(country_counts).map((val) => {
                    const randomNum = () => Math.floor(Math.random() * (235 - 52 + 1) + 52);
                    const randomRGB = () => `rgb(${randomNum()}, ${randomNum()}, ${randomNum()})`;
                    return randomRGB();
                }),
                data: Object.values(country_counts).map((val) => {
                    return val/sum_of_counts * 100;
                })
            }
        ]
    }
})

const chartOptions = computed(() => {
    return {
        plugins: {
            legend: {
                display: false
            }
        },
        responsive: true,
        maintainAspectRatio: false
    }
})

</script>


<template>
    <div class="flex w-full">
        <Pie
                id="firm_country_chart"
                :options="chartOptions"
                :data="chartData"
        />
    </div>

</template>

