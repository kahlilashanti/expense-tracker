<template>
    <h3>History</h3>

    <!-- we want to loop through the array and inject the result into the list items in the UI -->
    
    <ul class="list" id="list">
        <li v-for="transaction in transactions" 
        :key="transaction.id" :class="transaction.amount < 0 ? 'minus': 'plus'">
            {{ transaction.text }}<span>${{ transaction.amount }}</span><button @click="deleteTransaction(transaction.id)"
            class="delete-btn">x
            </button></li>
    </ul>
    
</template>

<script setup>
import { defineProps } from 'vue';


const emit = defineEmits(['transactionDeleted']);

const props = defineProps({
    transactions: {
        type: Array,
        required: true
    }
});

const deleteTransaction =(id) => {
    emit('transactionDeleted', id);
}


// this is the Options API version
// export default {
//     data() {
//         return {
//             transactions: [
//                 {id: 1, text: 'Flowers', amount: -19.99},
//                 {id: 2, text: 'Salary', amount: 209.99},
//                 {id: 3, text: 'Book', amount: -12.99},
//                 {id: 4, text: 'Squirt Gun', amount: 16},
//             ],
//         };
//     },
//


     
           
    

</script>