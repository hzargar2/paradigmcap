<script setup lang="ts">

const router = useRouter();
const id = router.currentRoute.value.params.id;
const { data: trades } = await useFetch(`https://paradigmapi.pythonanywhere.com/api/clients/${id}/trades`);

let tickers = [...new Set(trades.value.map((value) => value['full_ticker']))];
let selected_ticker= ref(null);

let sides = [...new Set(trades.value.map((value) => value['side']))];
let selected_side= ref(null);

let commission_types = [...new Set(trades.value.map((value) => value['commission_type']))];
let selected_commission_type= ref(null);

let selected_trades = reactive(trades.value);

let total_commission = trades.value.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0);

let average_commission = Math.round(((trades.value.map((trade) => {
    return trade['commission'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / trades.value.length) + Number.EPSILON) * 100) / 100


const filterTrades = (event) => {

    let filtered_trades = trades.value;
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
  <div class="flex flex-col w-full h-full space-y-8 p-6">
      <span class="mx-auto ml-0 text-3xl font-medium">Trades</span>
      <div class="flex w-full h-full space-x-12">
          <div class="flex flex-col w-1/4 my-auto h-full space-y-6">
              <div class="flex flex-col w-full h-full">
                  <span><span class="font-semibold">Total Commission:</span> ${{total_commission}}</span>
                  <span><span class="font-semibold">Average Commission:</span> ${{average_commission}} / trade</span>
              </div>
              <form class="max-w-sm">
                  <label for="selected_ticker" class="block mb-2 text-sm font-medium text-gray-900">Ticker</label>
                  <select @change="filterTrades($event)" v-model="selected_ticker" id="selected_ticker" class="block w-full p-2 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                      <option selected :value="null">All</option>
                      <option v-for="item in tickers" :value="item">{{item}}</option>
                  </select>
              </form>

              <form class="max-w-sm">
                  <label for="selected_side" class="block mb-2 text-sm font-medium text-gray-900">Side</label>
                  <select @change="filterTrades($event)" v-model="selected_side" id="selected_side" class="block w-full p-2 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                      <option selected :value="null">All</option>
                      <option v-for="item in sides" :value="item">{{item}}</option>
                  </select>
              </form>

              <form class="max-w-sm">
                  <label for="selected_commission_type" class="block mb-2 text-sm font-medium text-gray-900">Commission Type</label>
                  <select @change="filterTrades($event)" v-model="selected_commission_type" id="selected_commission_type" class="block w-full p-2 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500">
                      <option selected :value="null">All</option>
                      <option v-for="item in commission_types" :value="item">{{item}}</option>
                  </select>
              </form>

          </div>
          <div class="h-[600px] rounded-2xl p-4 flex w-3/4 my-auto border border-gray-200">
            <TradeChart :trades="selected_trades"/>
          </div>
      </div>
  </div>

</template>
