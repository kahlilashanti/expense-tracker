<template>
    <Header />
    <div class="container">
        <Balance :total="+total"/>
        <IncomeExpenses :income="+income" :expenses="+expenses"/>
        <TransactionList :transactions="transactions"  @transactionDeleted="handleTransactionDeleted"/>
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
</template>


<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import {useToast} from 'vue-toastification';
//to make it reactive use ref
import { ref, computed, onMounted } from 'vue';

//initialize toast
const toast = useToast();


//Composition API version
const transactions= ref([]);

//lifecycle method that checks localstorage to see if any transactions exist and if so, show them
onMounted(() => {
    //check localstorage, and only strings are in localstorage and this will turn it back into an array
    const savedTransactions = JSON.parse(localStorage.getItem
    ('transactions'));

    if(savedTransactions){
        transactions.value = savedTransactions;
    }
});


//get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

//get income
const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//get expenses
const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//add transaction
const handleTransactionSubmitted = (transactionData) => {
    // console.log(transactionData);
    //a helper function to create a unique ID
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text, 
        amount: transactionData.amount
    });
    //save to localStorage
    saveTransactionsToLocalStorage();


    //just to be sure the generate id function is working
    console.log(generateUniqueId())
    toast.success('Your transaction has been added');
};

//generate unique id
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
}

//delete transaction
const handleTransactionDeleted = (id) => {
    // console.log(id);
    transactions.value = transactions.value.filter((transaction) =>
    transaction.id !== id);

    //save remaining transactions to localStorage
    saveTransactionsToLocalStorage();

    toast.success('Transaction deleted');
}

//create a function that saves or removes from localstorage
    const saveTransactionsToLocalStorage = () => {
        localStorage.setItem('transactions', JSON.stringify(transactions.
        value));
    }

</script>