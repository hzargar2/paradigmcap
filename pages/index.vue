<script setup lang="ts">

const { data: clients } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients?limit=10000')

let top_10_clients = { ...clients.value }
top_10_clients.items.sort(({total_aum: a}, {total_aum: b}) => b-a);
top_10_clients = top_10_clients.items.slice(0, 10);

let total_aum = clients.value.items.map((client) => {
    return client['total_aum'];
}).reduce((pv, cv) => { return pv + cv; }, 0);

let average_firm_age = Math.round(((clients.value.items.map((client) => {
    return new Date().getFullYear() - client['year_established'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / clients.value.items.length) + Number.EPSILON) * 100) / 100

let average_tier = Math.round(((clients.value.items.map((client) => {
    return client['tier'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / clients.value.items.length) + Number.EPSILON) * 100) / 100

let countries = clients.value.items.map((client) => {
    return client['location'].split(",").map((item) => {
        return item.trim();
    })[1];
})

const { data: trades } = await useFetch(`https://paradigmapi.pythonanywhere.com/api/trades?limit=10000`);

let tickers = [...new Set(trades.value.items.map((value) => value['full_ticker']))];
let selected_ticker= ref(null);

let sides = [...new Set(trades.value.items.map((value) => value['side']))];
let selected_side= ref(null);

let commission_types = [...new Set(trades.value.items.map((value) => value['commission_type']))];
let selected_commission_type= ref(null);

let selected_trades = reactive(trades.value.items);

let total_commission = trades.value.items.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0);

let average_commission =  Math.round(((trades.value.items.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / clients.value.items.length) + Number.EPSILON) * 100) / 100

let average_commission_per_trade = Math.round(((trades.value.items.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / trades.value.items.length) + Number.EPSILON) * 100) / 100

const filterTrades = (event) => {

    let filtered_trades = trades.value.items;
    if (selected_side.value !== null){
        filtered_trades = filtered_trades.filter((trade) => trade['side'] === selected_side.value);
    }
    if (selected_ticker.value !== null){
        filtered_trades = filtered_trades.filter((trade) => trade['full_ticker'] === selected_ticker.value);
    }
    if (selected_commission_type.value !== null){
        filtered_trades = filtered_trades.filter((trade) => trade['commission_type'] === selected_commission_type.value);
    }

    selected_trades = filtered_trades;
}






const chartOptions = {
    chart: {
        id: "vuechart-example",
        height: "100%",
        maxWidth: "100%",
        type: "area",
        fontFamily: "Inter, sans-serif",
        dropShadow: {
            enabled: false,
        },
        toolbar: {
            show: false,
        },
    },
    tooltip: {
        enabled: true,
        x: {
            show: false,
        },
    },
    fill: {
        type: "gradient",
        gradient: {
            opacityFrom: 0.55,
            opacityTo: 0,
            shade: "#1C64F2",
            gradientToColors: ["#1C64F2"],
        },
    },
    dataLabels: {
        enabled: false,
    },
    stroke: {
        width: 6,
    },
    grid: {
        show: false,
        strokeDashArray: 4,
        padding: {
            left: 2,
            right: 2,
            top: 0
        },
    },
    series: [
        {
            name: "New users",
            data: [6500, 6418, 6456, 6526, 6356, 6456],
            color: "#1A56DB",
        },
    ],
    xaxis: {
        categories: ['01 February', '02 February', '03 February', '04 February', '05 February', '06 February', '07 February'],
        labels: {
            show: false,
        },
        axisBorder: {
            show: false,
        },
        axisTicks: {
            show: false,
        },
    },
    yaxis: {
        show: false,
    },
}



</script>

<template>
    <div class="flex flex-col w-full h-full p-6 pb-24">
        <span class="flex text-wrap ml-0 mx-auto justify-center text-3xl font-medium">Summary</span>

        <div class="flex flex-row w-full gap-x-4 h-full mx-auto p-6">
            <div class="flex flex-col h-full w-[40%] text-xl space-y-2 text-slate-700">

                <div class="w-full bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700">
                    <div class="sm:hidden">
                        <label for="tabs" class="sr-only">Select tab</label>
                        <select id="tabs" class="bg-gray-50 border-0 border-b border-gray-200 text-gray-900 text-sm rounded-t-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                            <option>Statistics</option>
                            <option>Top Clients</option>
                        </select>
                    </div>
                    <ul class="hidden text-sm font-medium text-center text-gray-500 divide-x divide-gray-200 rounded-lg sm:flex dark:divide-gray-600 dark:text-gray-400 rtl:divide-x-reverse" id="fullWidthTab" data-tabs-toggle="#fullWidthTabContent" role="tablist">
                        <li class="w-full">
                            <button id="stats-tab" data-tabs-target="#stats" type="button" role="tab" aria-controls="stats" aria-selected="true" class="inline-block w-full p-4 rounded-ss-lg bg-gray-50 hover:bg-gray-100 focus:outline-none dark:bg-gray-700 dark:hover:bg-gray-600">Statistics</button>
                        </li>
                        <li class="w-full">
                            <button id="about-tab" data-tabs-target="#top_clients" type="button" role="tab" aria-controls="top_clients" aria-selected="false" class="inline-block w-full p-4 bg-gray-50 hover:bg-gray-100 focus:outline-none dark:bg-gray-700 dark:hover:bg-gray-600">Top Clients</button>
                        </li>
                    </ul>
                    <div id="fullWidthTabContent" class="border-t border-gray-200 dark:border-gray-600">
                        <div class="hidden bg-white rounded-lg dark:bg-gray-800" id="stats" role="tabpanel" aria-labelledby="stats-tab">
                            <dl class="grid max-w-screen-xl grid-cols-2 gap-8 p-4 mx-auto text-gray-900 dark:text-white sm:p-8">

                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">${{total_aum }}M</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Total AUM</dd>
                                </div>
                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">{{clients.items.length }}</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Clients</dd>
                                </div>
                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">{{average_firm_age}} years</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Avg. Firm Age</dd>
                                </div>
                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">{{average_tier}}</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Avg. Firm Tier</dd>
                                </div>
                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">${{total_commission}}</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Total Commission</dd>
                                </div>
                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">${{average_commission}}</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Avg. Commission Per Client</dd>
                                </div>
                                <div class="flex flex-col items-center justify-center">
                                    <dt class="mb-2 text-xl font-extrabold">${{average_commission_per_trade}}</dt>
                                    <dd class="text-gray-500 text-center dark:text-gray-400 text-base">Avg. Commission Per Trade</dd>
                                </div>
                            </dl>
                        </div>
                        <div class="hidden p-4 bg-white rounded-lg md:p-8 dark:bg-gray-800" id="top_clients" role="tabpanel" aria-labelledby="top-clients-tab">
                            <h2 class="mb-5 text-2xl font-extrabold tracking-tight text-gray-900 dark:text-white">Top 10 by AUM</h2>
                            <!-- List -->
                            <ul role="list" class="space-y-4 text-base text-gray-500 dark:text-gray-400">
                                <li v-for="[index, client] of top_10_clients.entries()" class="flex space-x-2 rtl:space-x-reverse items-center">
                                    <span class="leading-tight">{{index + 1}}. {{ client['company_name'] }} (${{client['total_aum']}}M)</span>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div class="flex flex-col space-y-4 w-[60%] h-full bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 p-4 md:p-6">
                <span class="mx-auto text-xl font-medium">Firms by Country (%)</span>
                <div class="flex w-full h-full my-auto">
                    <FirmCountryChart :countries="countries"/>
                </div>
            </div>
        </div>

        <div class="flex w-full h-full space-x-12 pt-8">
            <div class="flex flex-col w-1/4 my-auto h-full">

                <form class="max-w-sm">
                    <label for="selected_ticker" class="block mb-2 text-sm font-medium text-gray-900">Ticker</label>
                    <select @change="filterTrades($event)" v-model="selected_ticker" id="selected_ticker" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                        <option selected :value="null">All</option>
                        <option v-for="item in tickers" :value="item">{{item}}</option>
                    </select>
                </form>

                <form class="max-w-sm">
                    <label for="selected_side" class="block mb-2 text-sm font-medium text-gray-900">Side</label>
                    <select @change="filterTrades($event)" v-model="selected_side" id="selected_side" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                        <option selected :value="null">All</option>
                        <option v-for="item in sides" :value="item">{{item}}</option>
                    </select>
                </form>

                <form class="max-w-sm">
                    <label for="selected_commission_type" class="block mb-2 text-sm font-medium text-gray-900">Commission Type</label>
                    <select @change="filterTrades($event)" v-model="selected_commission_type" id="selected_commission_type" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                        <option selected :value="null">All</option>
                        <option v-for="item in commission_types" :value="item">{{item}}</option>
                    </select>
                </form>

            </div>
            <div class="h-[600px] rounded-2xl p-4 flex w-3/4 my-auto border border-gray-200">
                <TradeChart :trades="selected_trades"/>
            </div>
        </div>


        <div class="max-w-sm w-full bg-white rounded-lg shadow dark:bg-gray-800 p-4 md:p-6">
            <div class="flex justify-between">
                <div>
                    <h5 class="leading-none text-3xl font-bold text-gray-900 dark:text-white pb-2">32.4k</h5>
                    <p class="text-base font-normal text-gray-500 dark:text-gray-400">Users this week</p>
                </div>
                <div
                    class="flex items-center px-2.5 py-0.5 text-base font-semibold text-green-500 dark:text-green-500 text-center">
                    12%
                    <svg class="w-3 h-3 ms-1" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 14">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13V1m0 0L1 5m4-4 4 4"/>
                    </svg>
                </div>
            </div>
            <ClientOnly>
                <apexchart
                    type="area"
                    :options="chartOptions"
                    :series="chartOptions.series"
                ></apexchart>
            </ClientOnly>
            <div class="grid grid-cols-1 items-center border-gray-200 border-t dark:border-gray-700 justify-between">
                <div class="flex justify-between items-center pt-5">
                    <!-- Button -->
                    <button
                        id="dropdownDefaultButton"
                        data-dropdown-toggle="lastDaysdropdown"
                        data-dropdown-placement="bottom"
                        class="text-sm font-medium text-gray-500 dark:text-gray-400 hover:text-gray-900 text-center inline-flex items-center dark:hover:text-white"
                        type="button">
                        Last 7 days
                        <svg class="w-2.5 m-2.5 ms-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
                        </svg>
                    </button>
                    <!-- Dropdown menu -->
                    <div id="lastDaysdropdown" class="z-10 hidden bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
                        <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownDefaultButton">
                            <li>
                                <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Yesterday</a>
                            </li>
                            <li>
                                <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Today</a>
                            </li>
                            <li>
                                <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Last 7 days</a>
                            </li>
                            <li>
                                <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Last 30 days</a>
                            </li>
                            <li>
                                <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Last 90 days</a>
                            </li>
                        </ul>
                    </div>
                    <a
                        href="#"
                        class="uppercase text-sm font-semibold inline-flex items-center rounded-lg text-blue-600 hover:text-blue-700 dark:hover:text-blue-500  hover:bg-gray-100 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700 px-3 py-2">
                        Users Report
                        <svg class="w-2.5 h-2.5 ms-1.5 rtl:rotate-180" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 6 10">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 9 4-4-4-4"/>
                        </svg>
                    </a>
                </div>
            </div>
        </div>








    </div>
</template>

