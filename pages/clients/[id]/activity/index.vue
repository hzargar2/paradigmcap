<script setup lang="ts">

const router = useRouter();
const id = router.currentRoute.value.params.id;
const { data: trades } = await useFetch(`https://paradigmapi.pythonanywhere.com/api/clients/${id}/trades`);

</script>

<template>
  <div class="flex flex-col w-full h-full space-y-16 p-6">
      <div class="flex flex-col w-full h-full space-y-8">
          <span class="mx-auto ml-0 text-3xl font-medium">Trades</span>
          <div class="flex w-full h-full">
              <TradeChart :trades="trades"/>
          </div>
      </div>
      <div class="flex flex-col w-full h-full space-y-8">
          <span class="mx-auto ml-0 text-3xl font-medium">Commissions</span>
          <div class="flex w-full h-full">
              <CommissionChart :trades="trades"/>
          </div>
      </div>

  </div>

</template>