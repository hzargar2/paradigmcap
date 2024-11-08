<script setup lang="ts">

const { data } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients')

console.log(data)
</script>

<template>
    <div class="flex w-full h-full">
        <div class="grid grid-cols-2 gap-8 w-full">
            <ClientCard v-for="client in data.items"
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

