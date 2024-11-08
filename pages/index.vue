<script setup lang="ts">

const { data: clients } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients')

let total_aum = clients.value.items.map((client) => {
    return client['total_aum'];
}).reduce((pv, cv) => { return pv + cv; }, 0);

let average_firm_age = clients.value.items.map((client) => {
    return new Date().getFullYear() - client['year_established'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / clients.value.items.length;

let average_tier = clients.value.items.map((client) => {
    return client['tier'];
}).reduce((pv, cv) => { return pv + cv; }, 0) / clients.value.items.length;

let countries = clients.value.items.map((client) => {
    return client['location'].split(",").map((item) => {
        return item.trim();
    })[1];
})

console.log(countries);

</script>

<template>
    <div class="flex flex-col w-full h-full space-y-8">
        <span class="flex text-wrap ml-0 mx-auto justify-center text-3xl font-medium">Summary</span>

        <div class="w-2/3 h-full grid grid-cols-2 mx-auto">
            <div class="flex flex-col h-fit my-auto w-full text-xl space-y-2 text-slate-700">
                <span><span class="font-semibold">Total AUM:</span> ${{total_aum }}M</span>
                <span><span class="font-semibold"># Clients:</span> {{clients.items.length }}</span>
                <span><span class="font-semibold">Average Firm Age:</span> {{average_firm_age}} years</span>
                <span><span class="font-semibold">Average Firm Tier:</span> {{average_tier}}</span>
            </div>
            <div class="flex flex-col space-y-4 h-full">
                <span class="mx-auto text-xl font-medium">Firm Country Distribution (%)</span>
                <div class="flex w-full h-[300px] my-auto">
                    <FirmCountryChart :countries="countries"/>
                </div>
            </div>
        </div>

    </div>
</template>

