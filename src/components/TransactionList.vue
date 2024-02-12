<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      transaction text
      <span>$0.0 </span>
      <button class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script>
export default {};
</script>
