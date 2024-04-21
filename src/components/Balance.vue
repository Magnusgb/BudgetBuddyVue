<template>
  <h4>Din Balance</h4>
      <h1 id="balance">{{ total }} DKK</h1>

</template>

<script setup>
  import { defineProps } from 'vue'
    const props = defineProps({
      total: {
        type: Number,
        required: true
      },
  });
</script>