<template>
    <div class="flex flex-col h-screen w-full">
        <nav class="w-full flex relative bg-white drop-shadow-md z-[2]">
            <div class="flex p-4 px-5 w-full justify-between mx-auto text-sm">
                <a href="/" class="w-auto flex items-center">
                    <img src="~/assets/logo-paradigm-dark.png" class="h-11" alt="Paradigm Capital Logo" />
                </a>
            </div>
        </nav>
        <!-- Markup shared across all pages, ex: NavBar -->
        <NuxtPage />
    </div>
</template>