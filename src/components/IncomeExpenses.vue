<template>
  <div class="inc-exp-container">
        <div>
          <h4>Indkomst</h4>
          <p id="money-plus" class="money plus">+{{ income }} DKK </p>
        </div>
        <div>
          <h4>Udgift</h4>
          <p id="money-minus" class="money minus">{{ expenses }} DKK</p>
        </div>
      </div>
</template>

<script setup>
  import { defineProps } from 'vue';
  const props = defineProps({
    income: {
      type: Number,
      required: true
    },
    expenses: {
      type: Number,
      required: true
    }
  });
</script>