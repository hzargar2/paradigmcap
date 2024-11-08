<script setup lang="ts">

const { data: clients } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients?limit=10000')

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

</script>

<template>
    <div class="flex flex-col w-full h-full space-y-16">
        <span class="flex text-wrap ml-0 mx-auto justify-center text-3xl font-medium">Summary</span>

        <div class="w-2/3 gap-x-4 h-full grid grid-cols-2 mx-auto">
            <div class="flex flex-col h-fit my-auto w-full text-xl space-y-2 text-slate-700">
                <span><span class="font-semibold">Total AUM:</span> ${{total_aum }}M</span>
                <span><span class="font-semibold"># Clients:</span> {{clients.items.length }}</span>
                <span><span class="font-semibold">Average Firm Age:</span> {{average_firm_age}} years</span>
                <span><span class="font-semibold">Average Firm Tier:</span> {{average_tier}}</span>
                <span><span class="font-semibold">Total Commission:</span> ${{total_commission}}</span>
                <span><span class="font-semibold">Average Commission:</span> ${{average_commission}} / client</span>
                <span><span class="font-semibold">Average Commission Per Trade:</span> ${{average_commission_per_trade}} / trade</span>
            </div>
            <div class="flex flex-col space-y-4 h-full">
                <span class="mx-auto text-xl font-medium">Firm Country Distribution (%)</span>
                <div class="flex w-full h-[300px] my-auto">
                    <FirmCountryChart :countries="countries"/>
                </div>
            </div>
        </div>

        <div class="flex w-full h-full space-x-12 pt-8">
            <div class="flex flex-col w-1/4 my-auto h-full">

                <form class="max-w-sm">
                    <label for="selected_ticker" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Ticker</label>
                    <select @change="filterTrades($event)" v-model="selected_ticker" id="selected_ticker" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                        <option selected :value="null">All</option>
                        <option v-for="item in tickers" :value="item">{{item}}</option>
                    </select>
                </form>

                <form class="max-w-sm">
                    <label for="selected_side" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Side</label>
                    <select @change="filterTrades($event)" v-model="selected_side" id="selected_side" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                        <option selected :value="null">All</option>
                        <option v-for="item in sides" :value="item">{{item}}</option>
                    </select>
                </form>

                <form class="max-w-sm">
                    <label for="selected_commission_type" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Commission Type</label>
                    <select @change="filterTrades($event)" v-model="selected_commission_type" id="selected_commission_type" class="block w-full p-2 mb-6 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                        <option selected :value="null">All</option>
                        <option v-for="item in commission_types" :value="item">{{item}}</option>
                    </select>
                </form>

            </div>
            <div class="bg-[#232D4B] h-[450px] rounded-2xl p-4 flex w-3/4 my-auto border border-gray-200">
                <TradeChart :trades="selected_trades"/>
            </div>
        </div>

    </div>
</template>

