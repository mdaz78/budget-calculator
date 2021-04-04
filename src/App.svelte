<script>
  import { setContext } from "svelte";

  import Navbar from "./Components/Navbar.svelte";
  import ExpenseList from "./Components/ExpenseList.svelte";
  import Totals from "./Components/Totals.svelte";
  import ExpenseForm from "./Components/ExpenseForm.svelte";

  import expensesData from "./expenses.js";

  let expenses = expensesData;

  $: total = expenses.reduce((sum, item) => (sum += item.amount), 0);

  // functions
  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  function removeAllExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    const expense = {
      id: Math.random() * Date.now(),
      name,
      amount,
    };

    expenses = [expense, ...expenses];
  }

  setContext("expense", { remove: removeExpense, add: addExpense });
</script>

<Navbar />

<main class="content">
  <ExpenseForm />
  <Totals title="Total Expenses" {total} />
  <ExpenseList {expenses} />
  {#if expenses.length > 0}
    <button class="btn btn-primary btn-block" on:click={removeAllExpenses}>Clear
      Expenses</button>
  {/if}
</main>
