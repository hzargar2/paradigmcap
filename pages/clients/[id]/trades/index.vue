<script setup lang="ts">

const { data } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients')

</script>

<template>
  <div>
      {{ $route.params.id }}
  </div>

</template>
