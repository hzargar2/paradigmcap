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

let total_commission = trades.value.items.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0);

let average_commission =  Math.round(((trades.value.items.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / clients.value.items.length) + Number.EPSILON) * 100) / 100

let average_commission_per_trade = Math.round(((trades.value.items.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / trades.value.items.length) + Number.EPSILON) * 100) / 100


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

    </div>
</template>

