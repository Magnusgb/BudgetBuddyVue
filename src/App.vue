<template>
    <Header />
    <div class="container">
        <Balance :total="+total"/>
        <IncomeExpenses :income="+income" :expenses="+expenses"/>
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
        <AddTransaction @add-transaction="handleSubmitted"/>
    </div>
</template>


<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import {useToast} from 'vue-toastification';

  import { ref, computed, onMounted } from 'vue';

  const toast = useToast();

  const transactions = ref([]);

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem
    ('transactions'));

    if (savedTransactions) {
      transactions.value = savedTransactions; 
    }
  });



  //Balance
  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc += transaction.amount;
    },
    0);
  });

  //Income 
  const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
  });


  //Expense
  const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
  });

  const handleSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount})
      saveTransactions ();
      toast.success('Transaktion tilføjet');
  }

  const generateUniqueId = () => {
    return Math.floor(Math.random() * 100000000);
  } 

  //Delete transaction
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    saveTransactions ();
    toast.success('Transaktion slettet');
  }


  //Save to local storage
  const saveTransactions = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }
</script>