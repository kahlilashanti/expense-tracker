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
import { ref, computed } from 'vue';

//initialize toast
const toast = useToast();


//Composition API version
const transactions= ref([
                {id: 1, text: 'Flowers', amount: -19.99},
                {id: 2, text: 'Salary', amount: 209.99},
                {id: 3, text: 'Book', amount: -12.99},
                {id: 4, text: 'Squirt Gun', amount: 22 },
            ]);


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

    toast.success('Transaction deleted');
}

</script>