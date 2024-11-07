<script setup lang="ts">


definePageMeta({
    layout: 'header'
})

</script>

<template>
    <div class="flex flex-col h-full w-full">
        <nav class="w-full flex fixed bg-white h-[64px] drop-shadow-md z-[2]">
            <div class="flex p-4 px-5 w-full justify-between mx-auto text-sm">
                <a href="/" class="w-auto flex items-center">
                    <img src="~/assets/logo-paradigm-dark.png" class="h-10" alt="Paradigm Capital Logo" />
                </a>
            </div>
        </nav>
        <!-- Markup shared across all pages, ex: NavBar -->
        <NuxtLayout name="dashboard">
            <NuxtPage/>
        </NuxtLayout>
    </div>
</template>