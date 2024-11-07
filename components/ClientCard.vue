<script setup lang="ts">

const props = defineProps(["id", "company_name", "company_type", "location", "contact_name", "contact_title", "contact_email", "contact_phone", "total_aum", "year_established", "tier", "risk_profile"])

</script>

<template>
  <div class="flex flex-col shadow-sm rounded-2xl border w-full p-4 space-y-4 hover:bg-slate-100 hover:cursor-pointer">
      <span class="mx-auto">{{ props.company_name }}</span>
      <div class="grid grid-rows-2 w-full text-slate-700">
          <span>AUM: ${{ props.total_aum }}M</span>
          <span>Type: {{ props.company_type }}</span>
          <span>Tier: {{ props.tier }}</span>
          <span>Contact: {{ props.contact_name }} ({{props.contact_title}})</span>
      </div>


  </div>
</template>
