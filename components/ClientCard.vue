<script setup lang="ts">

const props = defineProps(["id", "company_name", "company_type", "location", "contact_name", "contact_title", "contact_email", "contact_phone", "total_aum", "year_established", "tier", "risk_profile"])

const goToTradesPage = () => {
    window.location.href = `/clients/${props.id}/activity`
}
</script>

<template>
  <div class="flex flex-col shadow-sm rounded-2xl border w-full p-4 space-y-4 hover:bg-slate-100 hover:cursor-pointer" @click="goToTradesPage()">
      <span class="mx-auto font-semibold">{{ props.company_name }}</span>
      <div class="grid grid-rows-2 w-full text-slate-700">
          <span><span class="font-semibold">AUM:</span> ${{ props.total_aum }}M</span>
          <span><span class="font-semibold">Type:</span> {{ props.company_type }}</span>
          <span><span class="font-semibold">Tier:</span> {{ props.tier }}</span>
          <span><span class="font-semibold">Contact:</span> {{ props.contact_name }} ({{props.contact_title}})</span>
          <span><span class="font-semibold">Phone:</span> +{{ props.contact_phone }}</span>
      </div>
  </div>
</template>
