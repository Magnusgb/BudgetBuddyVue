<template>
   <h3>Tilføj ny transaktion</h3>
      <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text">Beskriv genstand</label>
          <input type="text" id="text" v-model="text" placeholder="Tilføj tekst..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Beløb <br />
            </label
          >
          <input type="text" id="amount" v-model="amount" placeholder="Indtast beløb..." />
        </div>
        <button class="btn">Tilføj transaktion</button>
      </form>
</template>

<script setup>
  import { ref } from 'vue';
  import {useToast} from 'vue-toastification';
import { parse } from 'vue/compiler-sfc';
  
  const text = ref('');
  const amount = ref('');

  const emit = defineEmits(['addTransaction']);

  const toast = useToast();

  const onSubmit = () => {
    if(!text.value || !amount.value) {
      toast.error('Udfyld venligst begge felter');
      return;
    }

    const transactionData = {
      text: text.value,
      amount: parseFloat(+amount.value)
    }

    emit('addTransaction', transactionData)

    text.value = '';
    amount.value = '';
    
  }
</script>