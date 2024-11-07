<script setup lang="ts">

// const { data } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients')


</script>

<template>
    <div class="flex flex-col w-full h-full">
        <span class="flex text-wrap m-auto justify-center text-4xl font-semibold">Summarize firm statistics here (ROE, AUM, Deal flow, etc..)</span>
        <span>{{data}}</span>
    </div>
</template>

