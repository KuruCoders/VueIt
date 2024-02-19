<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, ref, onMounted } from "vue";
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

// get incpme
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
// get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// handleTransactionSubmitted
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: Math.floor(Math.random() * 100000),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionsToLocal()
  toast.success("Transaction Added");
};

// handleTransactionDelete
const handleTransactionDelete = (transactionId) => {
  transactions.value = transactions.value.filter((transaction) => {
    return transaction.id !== transactionId;
  });
  saveTransactionsToLocal()
  toast.success("Deleted Transaction");
};
// saveToLocalStorage
const saveTransactionsToLocal = () => {
  localStorage.setItem('transactions',JSON.stringify(transactions.value))
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      @transactionDelete="handleTransactionDelete"
      :transactions="transactions"
    />
    <AddTransaction @transactionEvent="handleTransactionSubmitted" />
  </div>
</template>
