<script setup>
import { defineProps } from "vue";
const props = defineProps({
  income: {
    type: Number,
    required: true,
  },
  expenses: {
    type: Number,
    required: true,
  },
});
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">Rs {{ income }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">Rs {{ expenses }}</p>
    </div>
  </div>
</template>
