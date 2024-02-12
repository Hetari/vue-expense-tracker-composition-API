<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
  </div>
  <IncomeExpenses :income="+income" :expense="+expense" />
  <TransactionList
    :transaction="transactions"
    @delete-transaction="deleteTransaction"
  />
  <AddTransaction @-add-transaction="handleTransition" />
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpenses from "@/components/IncomeExpenses.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";
import { useToast } from "vue-toastification";
const toast = useToast();

const transactions = ref([]);
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transition) => transition.amount > 0)
    .reduce((acc, transition) => {
      return acc + transition.amount;
    }, 0);
});

const expense = computed(() => {
  return transactions.value
    .filter((transition) => transition.amount < 0)
    .reduce((acc, transition) => {
      return acc + transition.amount;
    }, 0);
});

const handleTransition = (data) => {
  transactions.value.push({
    id: generateId,
    text: data.text,
    amount: data.amount,
  });
  saveTransactionsToLocalStorage();
};

const generateId = () => {
  return Math.floor(Math.random() * 1_000_000);
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((obj) => obj.id !== id);
  toast.success("Transaction deleted!");
  saveTransactionsToLocalStorage();
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
