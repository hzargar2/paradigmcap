<script setup lang="ts">

const router = useRouter();
const id = router.currentRoute.value.params.id;
const { data: trades } = await useFetch(`https://paradigmapi.pythonanywhere.com/api/clients/${id}/trades`);

</script>

<template>
  <div class="flex flex-col w-full h-full space-y-8 p-6">
      <span class="mx-auto ml-0 text-3xl font-medium">Trades</span>
      <div class="flex w-full h-full">
          <TradeChart :trades="trades"/>
      </div>
  </div>

</template>


<!--<div class="flex flex-col w-1/4 my-auto h-full space-y-6">-->
<!--<div class="flex flex-col w-full h-full">-->
<!--    <span><span class="font-semibold">Total Commission:</span> ${{total_commission}}</span>-->
<!--    <span><span class="font-semibold">Average Commission:</span> ${{average_commission}} / trade</span>-->
<!--</div>-->
<!--</div>-->