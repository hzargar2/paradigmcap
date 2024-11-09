<script setup lang="ts">

import {toRefs} from "#imports";

const { data: clients } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients?limit=10000')

let filtered_clients = toRef(clients.value.items);

let search_input: string;

let filterClients = () => {
    if (search_input){
        filtered_clients.value = clients.value.items.filter((client) => client['company_name'].toLowerCase().includes(search_input.toLowerCase()) || client['contact_name'].toLowerCase().includes(search_input.toLowerCase()));
    }
}

</script>

<template>
    <div class="flex flex-col w-full h-full space-y-8 p-6">

        <form class="max-w-lg mx-auto w-full" onkeydown="return event.key !== 'Enter'">
            <label for="default-search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
            <div class="relative">
                <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
                    <svg class="w-4 h-4 text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
                    </svg>
                </div>
                <input v-on:input="filterClients" v-model="search_input" type="search" id="default-search" class="block w-full p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Search clients by company name or contact name..." required />
            </div>
        </form>

        <div class="grid grid-cols-2 gap-8 w-full">
            <ClientCard v-for="client in filtered_clients"
                        v-bind:key="client['company_name']"
                        :company_name="client['company_name']"
                        :total_aum="client['total_aum']"
                        :id = "client['id']"
                        :company_type="client['company_type']"
                        :location="client['location']"
                        :contact_name="client['contact_name']"
                        :contact_title="client['contact_title']"
                        :contact_email="client['contact_email']"
                        :contact_phone="client['contact_phone']"
                        :year_established="client['year_established']"
                        :tier="client['tier']"
                        :risk_profile="client['risk_profile']"
            />
        </div>
    </div>

</template>

