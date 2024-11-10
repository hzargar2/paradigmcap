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


let pie_options = computed(() => {
    return {
        series: Object.values(country_counts).map((val) => {
            return val/sum_of_counts * 100;
        }),
        chart: {
            id: "pie-chart",
            height: "100%",
            width: "100%",
            type: 'pie',
        },
        labels: Object.keys(country_counts),
        responsive: [{
            options: {
                legend: {
                    position: 'bottom'
                }
            }
        }]
    };
})

</script>


<template>
    <div class="flex flex-col w-full h-full">
        <ClientOnly>
            <apexchart
                type="pie"
                :options="pie_options"
                :series="pie_options.series"
            ></apexchart>
        </ClientOnly>
    </div>

</template>

