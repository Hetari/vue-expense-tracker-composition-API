<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        v-model="amount"
        type="text"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const text = ref("");
const amount = ref("");
const emit = defineEmits(["AddTransaction"]);

function isNumber(value) {
  if (typeof value !== "number") {
    return false;
  }

  return !isNaN(value) && Number.isFinite(value);
}

const onSubmit = () => {
  if (text.value.trim() === "" || amount.value.trim() === "") {
    toast.error("Please fill in all fields");
    return;
  }

  if (!isNumber(+amount.value)) {
    toast.warning("You should write a valid number in the amount filed!");
    amount.value = "";
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit("AddTransaction", transactionData);
  text.value = "";
  amount.value = "";
  toast.success("Transaction added successfully");
};
</script>
