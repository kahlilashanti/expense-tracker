<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..."/>
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
            (negative-expense, positive-income)</label>
            <input type="text" id="amount" v-model="amount" placeholder="Enter amount...">
        </div>
        <button class="btn">Add Transaction</button>
    </form>
</template>

<script setup>
import {ref} from 'vue';
import {useToast} from 'vue-toastification';

    const text = ref('');
    const amount = ref('');

    const emit = defineEmits(['transactionSubmitted']);

    //initialize toast
    const toast = useToast();

    const onSubmit = () => {
        if(!text.value || !amount.value) {
            toast.error('No empty fields allowed')
            return;
        }
        //this is the event we emit or listen for in App.vue when the form is submitted
        const transactionData = {
            text: text.value,
            amount: parseFloat(amount.value)
        }

        emit('transactionSubmitted', transactionData);

        // console.log(text.value, amount.value);

        text.value = '';
        amount.value = '';
    }
</script>