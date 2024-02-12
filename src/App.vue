<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
  </div>
  <IncomeExpenses :income="+income" :expense="+expense" />
  <TransactionList :transaction="transaction" />
  <AddTransaction @-add-transaction="handleTransition" />
</template>

<script setup>
import { ref, computed } from "vue";

import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpenses from "@/components/IncomeExpenses.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";

const transaction = ref([
  { id: 0, text: "Flower", amount: -19.99 },
  { id: 1, text: "Salary", amount: 300 },
  { id: 2, text: "Book", amount: -10 },
  { id: 3, text: "Camera", amount: 150 },
]);

const total = computed(() => {
  return transaction.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transaction.value
    .filter((transition) => transition.amount > 0)
    .reduce((acc, transition) => {
      return acc + transition.amount;
    }, 0);
});

const expense = computed(() => {
  return transaction.value
    .filter((transition) => transition.amount < 0)
    .reduce((acc, transition) => {
      return acc + transition.amount;
    }, 0);
});

const handleTransition = (data) => {
  transaction.value.push({
    id: generateId,
    text: data.text,
    amount: data.amount,
  });
};

const generateId = () => {
  return Math.floor(Math.random() * 1_000_000);
};
</script>
